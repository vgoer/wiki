<center>CatchAdmin</center>





[toc]









## CatchAdmin

> catchAdmin: 后台管框架。[catch](https://catchadmin.com/)
>
> [github](https://github.com/jaguarjack/catch-admin)







### 1. 搭建

> 为了方便环境，我直接使用docker了 [doc](https://catchadmin.com/docs/3.0/catchadmin/install)
>
> 环境要求： 
>
> - PHP >= 8.1+
> - Nginx
> - Mysql >= 5.7

```shell
git clone https://github.com/jaguarjack/catch-admin 

# 创建test网络
docker network create test

# 启动容器
docker-compose up -d
```

> 作者的这个compose文件有问题，用我们自己的 [github](https://github.com/vgoer/dockerLaravel.git)

```shell
1. 下载
git clone https://github.com/JaguarJack/catch-admin.git

2. 安装composer  和 依赖
composer install

3. 安装 node  和 pnpm 安装依赖
npm i pnpm -g 
pnpm i

4. 安装后台  按照提示输入对应信息即可
php artisan catch:install 

5. 启动后台
php artisan serve

6. 启动前端项目
npm dev
```





















