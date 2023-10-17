<center>Gin数据库GORM</center>





[toc]









## GORM

> 许多框架都会引入 ORM 模型来表示模型类和数据库表的映射关系，这一篇将使用 [gorm](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fgo-gorm%2Fgorm) 作为 ORM 库，它遵循了 ActiveRecord（模型与数据库表一一对应） 模式，并且提供了强大的功能，例如模型关联、关联预加载、数据库迁移等，更多内容查看[官方文档](https://gorm.io/docs/)







### 1. 安装

```shell
go get -u gorm.io/gorm

# GORM 官方支持 sqlite、mysql、postgres、sqlserver
go get -u gorm.io/driver/mysql 
```





### 2. 定义配置项

> 新建 `config/database.go` 文件，自定义配置项

```go
package config

type Database struct {
    Driver string `mapstructure:"driver" json:"driver" yaml:"driver"`
    Host string `mapstructure:"host" json:"host" yaml:"host"`
    Port int `mapstructure:"port" json:"port" yaml:"port"`
    Database string `mapstructure:"database" json:"database" yaml:"database"`
    UserName string `mapstructure:"username" json:"username" yaml:"username"`
    Password string `mapstructure:"password" json:"password" yaml:"password"`
    Charset string `mapstructure:"charset" json:"charset" yaml:"charset"`
    MaxIdleConns int `mapstructure:"max_idle_conns" json:"max_idle_conns" yaml:"max_idle_conns"`
    MaxOpenConns int `mapstructure:"max_open_conns" json:"max_open_conns" yaml:"max_open_conns"`
    LogMode string `mapstructure:"log_mode" json:"log_mode" yaml:"log_mode"`
    EnableFileLogWriter bool `mapstructure:"enable_file_log_writer" json:"enable_file_log_writer" yaml:"enable_file_log_writer"`
    LogFilename string `mapstructure:"log_filename" json:"log_filename" yaml:"log_filename"`
}
```

> `config/config.go` 添加 `Database` 成员属性

```go
package config

type Configuration struct {
    App App `mapstructure:"app" json:"app" yaml:"app"`
    Log Log `mapstructure:"log" json:"log" yaml:"log"`
    Database Database `mapstructure:"database" json:"database" yaml:"database"`
}

```

> `config.yaml` 增加对应配置项

```yaml
database:
  driver: mysql # 数据库驱动
  host: 127.0.0.1 # 域名
  port: 3306 # 端口号
  database: gin_react # 数据库名称
  username: root # 用户名
  password: root123123 # 密码
  charset: utf8mb4 # 编码格式
  max_idle_conns: 10 # 空闲连接池中连接的最大数量
  max_open_conns: 100 # 打开数据库连接的最大数量
  log_mode: info # 日志级别
  enable_file_log_writer: true # 是否启用日志文件
  log_filename: sql.log # 日志文件名称

```







### 3. 自定义Logger(文件记录)

> `gorm` 有一个默认的 [logger](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fgo-gorm%2Fgorm%2Fblob%2Fmaster%2Flogger%2Flogger.go) ，由于日志内容是输出到控制台的，我们需要自定义一个写入器，将默认`logger.Writer` 接口的实现切换为自定义的写入器，上一篇引入了 `lumberjack` ，将继续使用它
>
> 
>
> 新建 `bootstrap/db.go` 文件，编写 `getGormLogWriter` 函数：

```go
package bootstrap

import (
	"io"
	"log"
	"os"
	"strconv"
	"time"
	"vgoer/gin_api/app/models"
	"vgoer/gin_api/global"

	"go.uber.org/zap"
	"gopkg.in/natefinch/lumberjack.v2"
	"gorm.io/driver/mysql"
	"gorm.io/gorm"
	"gorm.io/gorm/logger"
)


// 自定义 gorm Writer
func getGormLogWriter() logger.Writer {
	var writer io.Writer

	// 是否启用日志文件
	if global.App.Config.Database.EnableFileLogWriter {
		// 自定义 Writer
		writer = &lumberjack.Logger{
			Filename:   global.App.Config.Log.RootDir + "/" + global.App.Config.Database.LogFilename,
			MaxSize:    global.App.Config.Log.MaxSize,
			MaxBackups: global.App.Config.Log.MaxBackups,
			MaxAge:     global.App.Config.Log.MaxAge,
			Compress:   global.App.Config.Log.Compress,
		}
	} else {
		// 默认 Writer
		writer = os.Stdout
	}
	return log.New(writer, "\r\n", log.LstdFlags)
}

```

> 接下来，编写 `getGormLogger` 函数， 切换默认 `Logger` 使用的 `Writer`

```go
func getGormLogger() logger.Interface {
    var logMode logger.LogLevel

    switch global.App.Config.Database.LogMode {
    case "silent":
        logMode = logger.Silent
    case "error":
        logMode = logger.Error
    case "warn":
        logMode = logger.Warn
    case "info":
        logMode = logger.Info
    default:
        logMode = logger.Info
    }

    return logger.New(getGormLogWriter(), logger.Config{
        SlowThreshold:             200 * time.Millisecond, // 慢 SQL 阈值
        LogLevel:                  logMode, // 日志级别
        IgnoreRecordNotFoundError: false, // 忽略ErrRecordNotFound（记录未找到）错误
        Colorful:                  !global.App.Config.Database.EnableFileLogWriter, // 禁用彩色打印
    })
}
```

> 至此，自定义 Logger 就已经实现了，这里只简单替换了 `logger.Writer` 的实现，大家可以根据各自的需求做其它定制化配置





### 4. 初始化数据库

> 在 `bootstrap/db.go` 文件中，编写 `InitializeDB` 初始化数据库函数，以便于在 `main.go` 中调用

```go
package bootstrap

import (
    "gopkg.in/natefinch/lumberjack.v2"
    "gorm.io/driver/mysql"
    "gorm.io/gorm"
    "gorm.io/gorm/logger"
    "io"
    "jassue-gin/global"
    "log"
    "os"
    "strconv"
    "time"
)

func InitializeDB() *gorm.DB {
    // 根据驱动配置进行初始化
    switch global.App.Config.Database.Driver {
    case "mysql":
       return initMySqlGorm()
    default:
       return initMySqlGorm()
    }
}

// 初始化 mysql gorm.DB
func initMySqlGorm() *gorm.DB {
    dbConfig := global.App.Config.Database

    if dbConfig.Database == "" {
        return nil
    }
    dsn := dbConfig.UserName + ":" + dbConfig.Password + "@tcp(" + dbConfig.Host + ":" + strconv.Itoa(dbConfig.Port) + ")/" +
        dbConfig.Database + "?charset=" + dbConfig.Charset +"&parseTime=True&loc=Local"
    mysqlConfig := mysql.Config{
        DSN:                       dsn,   // DSN data source name
        DefaultStringSize:         191,   // string 类型字段的默认长度
        DisableDatetimePrecision:  true,  // 禁用 datetime 精度，MySQL 5.6 之前的数据库不支持
        DontSupportRenameIndex:    true,  // 重命名索引时采用删除并新建的方式，MySQL 5.7 之前的数据库和 MariaDB 不支持重命名索引
        DontSupportRenameColumn:   true,  // 用 `change` 重命名列，MySQL 8 之前的数据库和 MariaDB 不支持重命名列
        SkipInitializeWithVersion: false, // 根据版本自动配置
    }
    if db, err := gorm.Open(mysql.New(mysqlConfig), &gorm.Config{
        DisableForeignKeyConstraintWhenMigrating: true, // 禁用自动创建外键约束
        Logger: getGormLogger(), // 使用自定义 Logger
    }); err != nil {
        global.App.Log.Error("mysql connect failed, err:", zap.Any("err", err))
        return nil
    } else {
        sqlDB, _ := db.DB()
        sqlDB.SetMaxIdleConns(dbConfig.MaxIdleConns)
        sqlDB.SetMaxOpenConns(dbConfig.MaxOpenConns)
        return db
    }
}

func getGormLogger() logger.Interface {
    var logMode logger.LogLevel

    switch global.App.Config.Database.LogMode {
    case "silent":
        logMode = logger.Silent
    case "error":
        logMode = logger.Error
    case "warn":
        logMode = logger.Warn
    case "info":
        logMode = logger.Info
    default:
        logMode = logger.Info
    }

    return logger.New(getGormLogWriter(), logger.Config{
        SlowThreshold:             200 * time.Millisecond, // 慢 SQL 阈值
        LogLevel:                  logMode, // 日志级别
        IgnoreRecordNotFoundError: false, // 忽略ErrRecordNotFound（记录未找到）错误
        Colorful:                  !global.App.Config.Database.EnableFileLogWriter, // 禁用彩色打印
    })
}

// 自定义 gorm Writer
func getGormLogWriter() logger.Writer {
    var writer io.Writer

    // 是否启用日志文件
    if global.App.Config.Database.EnableFileLogWriter {
        // 自定义 Writer
        writer = &lumberjack.Logger{
            Filename:   global.App.Config.Log.RootDir + "/" + global.App.Config.Database.LogFilename,
            MaxSize:    global.App.Config.Log.MaxSize,
            MaxBackups: global.App.Config.Log.MaxBackups,
            MaxAge:     global.App.Config.Log.MaxAge,
            Compress:   global.App.Config.Log.Compress,
        }
    } else {
        // 默认 Writer
        writer = os.Stdout
    }
    return log.New(writer, "\r\n", log.LstdFlags)
}

```





### 5. 模型文件和数据迁移

> 新建 `app/models/common.go` 文件，定义公用模型字段

```go
package models

import (
    "gorm.io/gorm"
    "time"
)

// 自增ID主键
type ID struct {
    ID uint `json:"id" gorm:"primaryKey"`
}

// 创建、更新时间
type Timestamps struct {
    CreatedAt time.Time `json:"created_at"`
    UpdatedAt time.Time `json:"updated_at"`
}

// 软删除
type SoftDeletes struct {
    DeletedAt gorm.DeletedAt `json:"deleted_at" gorm:"index"`
}
```

> 新建 `app/models/user.go` 文件，定义 `User` 模型

```go
package models

type User struct {
    ID
    Name string `json:"name" gorm:"not null;comment:用户名称"`
    Mobile string `json:"mobile" gorm:"not null;index;comment:用户手机号"`
    Password string `json:"password" gorm:"not null;default:'';comment:用户密码"`
    Timestamps
    SoftDeletes
}
```

> 新建 `app/models/admin.go` 文件，定义 `Admin` 模型

```go
package models

type Admin struct {
	ID
	Username string `json:"username" grom:"not null; commen:用户名"`
	Password string `json:"password" grom:"not null; default:''; commen:密码"`
	Mobile   string `json:"mobile" gorm:"not null; index; comment:用户手机号"`
	Token    string `json:"token" gorm:"comment:token"`
	Timestamps
	SoftDeletes
}
```



> 在 `bootstrap/db.go` 文件中，编写数据库表初始化代码

```go
// 重要
func initMySqlGorm() *gorm.DB {
    // ...
    if db, err := gorm.Open(mysql.New(mysqlConfig), &gorm.Config{
        DisableForeignKeyConstraintWhenMigrating: true, // 禁用自动创建外键约束
        Logger: getGormLogger(), // 使用自定义 Logger
    }); err != nil {
        return nil
    } else {
        sqlDB, _ := db.DB()
        sqlDB.SetMaxIdleConns(dbConfig.MaxIdleConns)
        sqlDB.SetMaxOpenConns(dbConfig.MaxOpenConns)
        initMySqlTables(db)
        return db
    }
}

// 数据库表初始化
func initMySqlTables(db *gorm.DB) {
    err := db.AutoMigrate(
        models.User{},
    )
    if err != nil {
        global.App.Log.Error("migrate table failed", zap.Any("err", err))
        os.Exit(0)
    }
}
```





### 6. 定义全局变量DB

> 在 `global/app.go` 中，编写：

```go
package global

import (
	"vgoer/gin_api/config"

	"github.com/spf13/viper"
	"go.uber.org/zap"
	"gorm.io/gorm"
)

type Application struct {
	ConfigViper *viper.Viper
	Config      config.Configuration
	Log         *zap.Logger
	DB          *gorm.DB
}

var App = new(Application)
```





### 7. 测试

> 在 `main.go` 中调用数据库初始化函数

```go
package main

import (
	"net/http"
	"vgoer/gin_api/bootstrap"
	"vgoer/gin_api/global"

	"github.com/gin-gonic/gin"
)

func main() {

	// 初始化配置
	bootstrap.InitializeConfig()

	// 初始化日志
	global.App.Log = bootstrap.InitializeLog()
	global.App.Log.Info("log init success!!!")

	// 初始化数据库
	global.App.DB = bootstrap.InitializeDB()

	// 程序关闭前，释放数据库连接
	defer func() {
		if global.App.DB != nil {
			db, _ := global.App.DB.DB()
			db.Close()
		}
	}()

	server := gin.Default()

	// 测试路由
	server.GET("/ping", func(c *gin.Context) {
		c.String(http.StatusOK, "safdsafdsaf")
	})

	server.GET("/", func(ctx *gin.Context) {
		ctx.String(200, "hello pagedsdsafdsafsdfs")
	})

	// 启动服务器

	server.Run(":" + global.App.Config.App.Port)

}
```

> 启动 `main.go` ，由于我还没有创建 `go-test` 数据库，并且调整了 `logger.Writer` 为 `lumberjack`，所以会生成 `storage/logs/sql.log` 文件，文件内容如下：

```go
// slq日志
2023/10/17 14:49:44 D:/MyProject/go_react_test/gin_api2/bootstrap/db.go:63
[0.000ms] [rows:-] SELECT DATABASE()

2023/10/17 14:49:44 D:/MyProject/go_react_test/gin_api2/bootstrap/db.go:63
[0.522ms] [rows:1] SELECT SCHEMA_NAME from Information_schema.SCHEMATA where SCHEMA_NAME LIKE 'gin_react%' ORDER BY SCHEMA_NAME='gin_react' DESC,SCHEMA_NAME limit 1

```

