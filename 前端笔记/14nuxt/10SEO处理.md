<center>SEO处理</center>





[toc]











## SEO处理

> SEO处理







### 1. 页面添加SEO

```ts
// app.vue

useHead({
  title: "我是APP标题",
  meta:[
    {
      name: "description",
      content: "my desc",
    }
  ]
})
```



> About页面

```ts
useHead({
  // title: "关于",
  meta:[
    {
      name: "description",
      content: "user 页面 desc",
    },
    {
      name:"keywords",
      content: "关键字"
    }
  ],
  titleTemplate(titleChunk){
    // app 页面的标题
    console.log(titleChunk)
    return titleChunk ? `${titleChunk} - Site Title` : "Site title"
  }
})
```



