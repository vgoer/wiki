<center>Vulhub搭建</center>





[toc]





## Vulhub搭建

> 包含了各种 Web 应用、系统服务和组件的漏洞环境。[github](https://github.com/vulhub/vulhub)

```shell
# 下载项目
git clone https://github.com/vulhub/vulhub.git

# 进入某一个漏洞/环境的目录
cd flask/ssti

# 自动化编译环境
docker compose build

# 启动整个环境
docker compose up -d
```