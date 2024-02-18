<center>DaisyUi</center>





[toc]







## DaisyUi

> daisyUi[ui](https://daisyui.com/): 关于tailwind css最流行的组件库。 









### 1. 安装

1. 安装 daisyUI:

```undefined
npm i -D daisyui@latest
```

1. 然后，在你的 `tailwind.config.js` 文件里追加 daisyUI 的设置:

```js
module.exports = {
  //...
  plugins: [require("daisyui")],
}
```







### 2. 全局配置 

> 可以通过 `tailwind.config.js` 来配置 daisyUI 的配置。 默认配置:

```js
module.exports = {
  //...

  // add daisyUI plugin
  plugins: [require("daisyui")],

  // daisyUI config (optional - here are the default values)
  daisyui: {
    themes: false, // false: only light + dark | true: all themes | array: specific themes like this ["light", "dark", "cupcake"]
    darkTheme: "dark", // name of one of the included themes for dark mode
    base: true, // applies background color and foreground color for root element by default
    styled: true, // include daisyUI colors and design decisions for all components
    utils: true, // adds responsive and modifier utility classes
    prefix: "", // prefix for daisyUI classnames (components, modifiers and responsive class names. Not colors)
    logs: true, // Shows info about daisyUI version and used config in the console when building your CSS
    themeRoot: ":root", // The element that receives theme color CSS variables
  },

  //...
}
```

