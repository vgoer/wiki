<center>Next</center>





[toc]





## Next

> Next: React的web框架 [next](https://github.com/vercel/next.js) [next](https://nextjs.org/)
>
> 为什么选Next.js: `目前react，vue都是单页面应用，首屏加载过慢，而且不能SEO`





### 1. 优点

1. **首屏加载速度快**: 我们的内嵌场景比较丰富，因此比较`追求页面的一个首屏体验`

2. **SEO优化好**: SSR \ SSG \ ISR 支持`页面内容预加载`，提升了搜索引擎的友好性。
3. **内置特性易用且极致**: NextJS 内置 `getStaticProps`、`getServerSideProps`、`next/image`、`next/link`、`next/script`等特性





### 2. 安装

```shell
npx create-next-app@latest my-app
```

> `script`

```json
    "dev": "next dev",
    "build": "next build",
	// 生成环境运行
    "start": "next start",
	// 生成纯静态
    "export": "next export",
    "lint": "next lint"
```

> next 是动静结合的 所以网站被build`index.html`和`index.js`

```shell
npm run build # 后产生的.next需要放到服务器上
npm run start # 启动服务
```

























