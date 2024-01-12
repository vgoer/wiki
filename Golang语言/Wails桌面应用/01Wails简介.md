<center>Wails</center>







[toc]







## Wails

> 使用 Go 构建漂亮的跨平台应用程序 [wails](https://github.com/wailsapp/wails)
>
> Wails 是一个可让您使用 Go 和 Web 技术编写桌面应用的项目。
>
> 将它看作为 Go 的快并且轻量的 Electron 替代品。 您可以使用 Go 的灵活性和强大功能，结合丰富的现代前端，轻松的构建应用程序。









### 1. 安装 

> [安装](https://wails.io/zh-Hans/docs/gettingstarted/installation)

> Wails 有许多安装前需要的常见依赖项：
>
> - Go 1.18+
> - NPM (Node 15+)

```shell
# 安装 go 和 npm
go version
npm --version
```

> 安装Wails

```shell
go install github.com/wailsapp/wails/v2/cmd/wails@latest
```

> 检查安装

```shell
wails doctor

# 升级
wails setup
```







### 2. 创建项目

> 现在 CLI 已安装，您可以使用 `wails init` 命令生成一个新项目。

> 选择喜欢的框架： 
>
> ```shell
> svelte    react  vue   preact   lit   vanilla
> ```
>
> 

使用 JavaScript 生成一个 [Svelte](https://svelte.dev/) 项目:

```text
wails init -n myproject -t svelte
```

如果您更愿意使用 TypeScript:

```text
wails init -n myproject -t svelte-ts
```

> 项目布局

```shell
.
├── build/
│   ├── appicon.png
│   ├── darwin/
│   └── windows/
├── frontend/
├── go.mod
├── go.sum
├── main.go
└── wails.json
```





### 3. 启动

> 安装依赖和启动

```shell
wails dev 
```





### 4. 编译

> 从项目目录，运行 `wails build`。 这将编译您的项目并将构建的可用于生产的二进制文件保存在 `build/bin` 目录中。

```shell
wails build
```

> 多个平台

```shell
-platform   
darwin,darwin/amd64,darwin/arm64,darwin/universal,linux,linux/amd64,linux/arm64,linux/arm,windows,windows/amd64,windows/arm64,windows/386
```

