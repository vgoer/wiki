<center>Docusaurus文档</center>



[toc]







## Docusaurus文档

> Docusaurus 将帮助您**立即发布一个漂亮的文档网站**。
>
> [github ](https://github.com/facebook/docusaurus)[doc](https://docusaurus.io/docs)







### 1. 安装

```shell
mkdir -p ./doc
sudo chown -R 1001:1001 ./doc
sudo chmod -R 777 ./doc
```

> 初始化模板：

```shell
# 1. node环境
docker pull node:18

# 2. 启动环境初始化
docker run --rm -it \
  -v $PWD/docusaurus:/app \
  -w /app \
  -p 3000:3000 \
  node:18 \
  sh -c "npm config set registry https://registry.npmmirror.com && npx create-docusaurus@latest docusaurus classic --typescript  && npm install && npm run start"

# 初始化
npx create-docusaurus@latest my-website classic

# ts
npx create-docusaurus@latest my-website classic --typescript

# 启动
npm run start
```

