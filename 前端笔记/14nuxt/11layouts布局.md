<center>layouts布局</center>





[toc]







## layouts布局

> 布局





### 1. 布局

> 可以定义多个布局，在一个网站。

```vue
// layouts/default.vue

<style scoped lang='scss'>
header,
main,
footer {
    border: 1px solid blue;
}
</style>
<!-- layout -->
<template>
    <header>
        <ul>
            <li>
                <NuxtLink to="/">首页</NuxtLink>
            </li>
            <li>
                <NuxtLink to="/login">登陆</NuxtLink>
            </li>
            <li>
                <NuxtLink to="/about">关于</NuxtLink>
            </li>
            <li>
                <NuxtLink to="/user">用户页面</NuxtLink>
            </li>
        </ul>
    </header>
    <main>
        // 插槽
        <slot></slot>
    </main>
    <AppFooter />
</template>
<script setup lang='ts'>


</script>
```

> app页面

```vue
<template>
  <nuxt-layout>
    <nuxt-page></nuxt-page>
  </nuxt-layout>
</template>

```

> 登陆页面等一些不需布局的页面

```vue

<script lang="ts" setup>

definePageMeta({
  // 关闭布局
  layout: false
})
</script>
```





