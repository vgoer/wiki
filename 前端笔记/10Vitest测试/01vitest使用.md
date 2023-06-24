
<center>vitest测试框架</center>





[toc]





## Vitest

> 一个 Vite 原生的单元测试框架。非常的快！ [vitest](https://cn.vitest.dev/) [github](https://github.com/vitest-dev/vitest)



### 1. 安装

```shell
# 创建和安装
npm init vite@latest vue-vitest

# vitest
pnpm i 
pnpm i -D vitest
```



### 2. 使用

> 使用  安装插件 `Vitest`

```tsx
// 所有单元测试的代码统一目录 tests 统一命名为 index.spec.ts或者 index.test.ts

import { test, expect } from 'vitest'
// 测试 1+1 ?= 2
test('first', () => {
    expect(1+1).toBe(2)
})

// 在 package.json中配置脚本 sciprt
	    "test": "vitest"

// 不想使用watch 模式 
npx vitest run
```

> 测试其他文件库

```tsx
// add.ts
/**
 * 
 * 获取两数之和
 * @param num1 number 参数一
 * @param num2 number 参数二
 * @returns  number
 */
export function get_sum(num1:number, num2:number) :number {
    return num1+num2
}
```

```tsx
// 测试
import {  test, expect } from 'vitest'
import { get_sum } from '../src/add'
import Hello from '../src/components/HelloWorld.vue'

test('firest', () => {
    expect(1+1).toBe(2)
})

// 第二个测试
test('add test',() => {
    expect(get_sum(10,10)).toBe(20)
})

// 测试vue组件
test('vue test',() => {
    console.log(Hello)
})
```





### 3. 配置

> 可以直接在 `vite.config.ts`里面配置 `vitest`
>
> 也可以单独配置到`vitest.config.ts`里面，不建议这样

```ts
// 导入 类型
/// <reference types="vitest" />

export default defineConfig({

  test:{
	// test配置
  },
  plugins: [vue()],
})

```

1. 全局导入vitest

```ts
//  globals ture
	test:{
        globals:true,
    },

// 2. 配置 tsconfig.json
 	// types
    "types": [
      "vitest/globals"
    ]
```





### 4. 测试单文件组件

> 测试`.vue`文件

> `tps:` vue3找不到： 无法找到模块“xxx.vue”的声明文件 xxx隐式拥有 “any“ 类型。

```tsx
// typescript 只能理解 .ts 文件，无法理解 .vue文件
// 解决方法：在项目根目录或 src 文件夹下创建一个后缀为 XXX.d.ts 的文件，并写入以下内容：
// types.d.ts
declare module '*.vue'{
    import { ComponentOptions } from 'vue'
    const componentOptions: ComponentOptions
    export default componentOptions
}
```

> 测试打印组件

```tsx
import { test, expect } from 'vitest'
import myBtn from './MyBtn.vue'

test('vue-test',() => {
    console.log(myBtn)
})
```

> 现在拿到的只是一个普通的对象，需要挂载才是对象

```tsx
// vue 官方
// pnpm i @vue/test-utils -D
// pnpm i jsdom -D  // jsdom 上面工具需要用到
```

> 配置`vitest`

```tsx
  test:{
    environment:"jsdom"
  },
```

```tsx
// 测试
import { test, expect } from 'vitest'
import myBtn from './MyBtn.vue'
import { mount } from '@vue/test-utils'

test('vue-test',() => {
    console.log(myBtn)
    const comBtn = mount(myBtn)
    //comBtn 对象又很多的api供给我们测试
    // 测试 组件里面的 text 内容是不是 hello
    expect((comBtn.text())).toContain("hello")
    
    // 测试 input  output
    // api  pinan 等都可以测试
})
```





### 5. 测试jsx文件

> `.jsx/.tsx` 对这个文件进行测试 [vue3+tsx](https://juejin.cn/post/6975398843173044255)

```tsx
// myInput.tsx
import { defineComponent } from 'vue';

export default defineComponent({
    setup(){
        return () => <div>hello</div>;
    }
});
```

> 一个完整的`tsx`组件

```tsx
// TsxTest.tsx
import { FunctionalComponent as FC, defineComponent, reactive, onMounted } from 'vue';

// 无状态组件
const FcNode: FC<{ data: number }> = ({ data }) => {
    return (
        <>
        <hr />
        <div>子组件：
            {data < 10 ? <span>{data}</span> : <h1>{data}</h1>}
        </div>
        </>
    )
};
// 状态组件需要使用 defineComponent
export default defineComponent({
    name: 'TsxTest',
    setup() {
        const data = reactive({ count: 0 });

        onMounted(() => {
        	data.count = 5;
        });

        const clickHandler = () => data.count++

        return () => (
        <>
            <span style={{ marginRight: '10px' }}>{ data.count }</span>
            <button onClick={clickHandler}>+</button>
            <FcNode data={data.count}/>
        </>
        )
    }
})
```

> 测试发现报错了，默认当为ssr环境了 `TypeError: Cannot read properties of undefined (reading 'modules')`

```ts
// 配置vite.config.ts
  test:{
    environment:"jsdom",
    transformMode:{
      // 遇到tsx当为web环境，不是ssr环境
      web:[/.tsx$/]
    }
  },
```

> 牛皮，测试跑起来了

```tsx
import { test } from 'vitest'
import { mount } from '@vue/test-utils'
import Input from './MyInput.tsx'


test('tsx-test', () => {
    console.log(Input)
    mount(Input)
})
```

