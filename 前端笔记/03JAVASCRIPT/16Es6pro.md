---
title: 16Es6pro
description: 
published: 1
date: 2023-05-29T04:14:15.483Z
tags: 
editor: markdown
dateCreated: 2023-05-15T17:21:13.938Z
---

<center>Es6</center>





[toc]



## 

> es6常用的语法





### 0.基本

```js
// 模版字符串
let count = 1;
let str = `count为 ${count}`

// let const 解决了 var 变量提升和没有块级作用域
// let 变量 const 常量
let str = "js"
const PI = 3.14

// 箭头函数 
// 一句话箭头
const add = num => num+num;
const conArr2 = (a,b,c) => console.log(a,b,c)
conArr2(1,2,3)

// 参数变为一个数组 ...args
const conArr = (...args) => {
    console.log(args)
}
conArr(1,2,3,4) //  [1, 2, 3, 4]

// 只循环值
const arr = [1,2,3,4,5]
for(let item of arr){
    console.log(item)
}
```





### 1. 解构赋值

> 解构很常用的 [bolg](https://juejin.cn/post/7073787096036540446#heading-0)

```js
// 变量
let [a,b,c] = [1,2,['aa','bbbb']]
console.log(a,b,c)

// 对象
const persion = {name:'goer',age:10,sex:'boy',lover:'code,ball,and so on'}
let {name:myName,age:myAge,sex,lover} = persion
console.log(myName,myAge,sex,lover)

// 字符串
const [s1,s2,...s3] = 'abAAA'
console.log(s1,s2,s3)  // a b (3) ['A', 'A', 'A']
```

> 上面的`...s3`的为什么可以是一个数组呢？

```js
// 解构语法里面的 展开运算符 
// 把数据拆开 一个一个的计算

// 数组展开
const arr = [1,2,3,4,5];
console.log(...arr) // 1  2  3

// 对象展开
const obj = {a:'aa',b:'bb'}
const newObj = {...obj,c:'ccc'}
console.log(newObj)

// 数组合并
const arr1 = ['aa','AA'];
const arr2 = ['bb','BB']
const arr3 = [...arr1,...arr2]
console.log(arr3)
```



### 2.面向对象

> es5和es6面向对象的实现

```js
// es5
function Compouter(name,age){

    this.name = name
    this.age = age
}
// 添加方法
Compouter.prototype.getName = function(){
    console.log(this.name)
}
const com1 = new Compouter('hp',10)
com1.getName()
```

> es6

```js
// es6 
class Compouter{
    constructor(name,age){
        this.name = name
        this.age  = age
    }

    sayName(){
        console.log(this.name)
    }

    sayAge = () => {
        setTimeout(() => {
            console.log(this.age)
        },1000)
    }

}
const com1 = new Compouter('hp',10)
com1.sayName()
```

> 继承属性

```js
// 继承父类所有方法和属性
// es6
class Compouter{
    constructor(name,age){
        this.name = name
        this.age  = age
    }

    sayName(){
        console.log(this.name)
    }

    sayAge = () => {
        setTimeout(() => {
            console.log(this.age)
        },1000)
    }

}

class Hp extends Compouter{

    constructor(name,age,love){
        super(name,age)
        this.love = love
    }

    sayLove = () => {
        console.log(this.love)
    }

}

const hp1 = new Hp('aa',10,'aaaAAA')
console.log(hp1)
```



### 3. 异步数据

> 最开始的解决方案，到后来的一步步优化

```js
// 同步和异步
// setInterval setTimeout 异步函数

let data = '我是异步数据'

getData = () => {

    setTimeout(() => {

        return data;
    },2000)
}

// 不能获取我们异步的数据
let resData = getData();
console.log(resData)  //undefined
```

> 回调函数

```js
// 解决方法 1. 回调函数接受返回值  如果需要太多次，就会回调地域
getOneData = (getFun) => {
    setTimeout(() => {
        getFun(data)
    }, 2000);
}

getOneData((e) => {
    console.log(e)
})
```

> `promise 和 async`

```js
// 解决方法 2. Promise 更优解
let proData = 'promise data'

getProData = () => {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve(proData)
        },2000)
    })
}

let pro1 = getProData()
pro1.then((data) => {
    console.log(data)
})


// 解决方法 3. async await 语法糖 目前最好的方法
getAsynData = async () => {
    let data1 = await getProData()
    console.log(data1)

    let data2 = await getProData()
    console.log(data2)

}
getAsynData()
```



### 4. Promise

> 上面有讲到使用`Promise`,现在我们看看具体用法

```js
//  基本用法
getData = () => {
    // resolve    .then     成功
    // reject     .catch    失败
    //            .finally  无论接口请求成功与否，都会走这里
    return new Promise((resolve,reject) => {

        // 模拟异步数据
        setTimeout(() => {
            const data = {code:1,msg:'yes',data:['aaa','bbb','ccc']}

            if(data.code === 1){
                resolve(data)
            }
            reject({code:'0',msg:'netwoke error'})
        },1000)

    })
}
getData()
    .then(data => {
        // 从 resolve 获取正常结果
        console.log(data)
    })
    // 在请求其他接口
    getData()
        .then(data => {
            console.log(data)
        })
    .catch(data => {
        // 从 reject 获取异步数据
        console.log(data)
    })
    .finally(() => console.log('都会执行'))

```

> 封装一个原生的`ajax`请求  `自己去封装一个post请求`

```js
// Promise.all()：并发处理多个异步任务，所有任务都执行成功，才能得到结果。
// Promise.race(): 并发处理多个异步任务，只要有一个任务执行成功，就能得到结果。

getAjaxData = (url) => {
    return new Promise((resolve,reject) => {

        let xhr = new XMLHttpRequest();
        xhr.onreadystatechange = () => {
            if(xhr.readyState != 4) return;
            if(xhr.readyState === 4 && xhr.status === 2000){

                resolve(xhr.responseText)
            }

            reject('network error')
        }

        // 发送请求
        xhr.open('get',url)
        xhr.send(null)
    })
}
let promise1  = getAjaxData('http://localhost:5500/a1?id=1')
let promise2  = getAjaxData('http://localhost:5500/a1?id=2')
let promise3  = getAjaxData('http://localhost:5500/a1?id=3')
Promise.all([promise1, promise2, promise3]).then(result => {
        console.log(result);
});
```





### 5. 处理异常

> `try...catch...` 处理异常的关键字

```js
    try{
        // 可能会抛出异常的代码
        let a =1;
        const res = a+b;
        console.log(res)
    }
    catch(e){
        // 异常处理代码
        console.error(e.message)  // b is not defined
    }
    finally{
        // 无论异常是否抛出都会执行的代码
        console.log('fil')
    }
```



### 6. Fetch

> `fetch()`是 XMLHttpRequest 的升级版，用于在 JavaScript 脚本里面发出 HTTP 请求。[blog](https://www.ruanyifeng.com/blog/2020/12/fetch-tutorial.html) 
>
> 参数说明 [blog](https://juejin.cn/post/7001769245457514532#heading-0)

```js
// 基本用法
getFetchData = async () => {
    // fetch(url,options)  
    let res = await fetch('./data.json',{
        // 在里面配置请求参数
        method:"get",

    }).then((response) => {
        // 响应体
        console.log(response)
        return response.json()

    }).then((data) => {
        console.log(data)
    })
}
```

> 发送一个`post`请求

```js
// post 请求
let url = "http://localhost:9000/api/test"
let loginData = {username:'admin',password:'admin....'}

fetch(url,{
    method:'POST',
    // 允许跨域
    mode:"cors",
    // 请求头
    headers:{
        'Content-Type':'application/json',
    },
    // body 请求体
    body: JSON.stringify(loginData)  // 数据必须是  `string` or {object}!
})
.then(data => console.log(data))
.catch(error => console.log(error))
```



### 7. 数组方法

> 新增了很多数组方法

1. map

> map() 方法创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果。

```js
let arr = [2,4,6,8]
const newMap = arr.map( x => x * 2)
console.log(newMap)  //  [4, 8, 12, 16]
```

2. filter

> `filter()` 过滤

```js
const filtered = [10,2,5,11,8].filter( num => num > 5)  
console.log(filtered) //  [10, 11, 8]

let myArray = [ '⚡️', '🔎', '🔑', '🔩' ];
let filteredArray = myArray.filter((element) => {
    return (element == '🔑' || element == '🔩')
});
console.log(filteredArray)

// 添加数组 三个参数 word,index,arr
const words = ['ball','run','text','go']
const filterWords = words.filter((word,index,arr) => {
    // 最后添加和最后删除
    arr.push('new words')
    arr.pop();
    console.log(word,index,arr)
})
```

3. forEach

> **`forEach()`** 方法对数组的每个元素执行一次给定的函数。

```js
const arr = ['aaa','bbb','cccc']
arr.forEach(x => console.log(x)) // aaa  bbb   ccc
```

4. includes

> **`includes()`** 方法用来判断一个数组是否包含一个指定的值. 如果包含则返回 `true`，否则返回 `false`。

```js
const arr = ['aaa','bbb','cccc']
console.log(arr.includes('aa'))
```

5. from

> **`Array.from()`** 静态方法从[可迭代](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols#可迭代协议)或[类数组](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Indexed_collections#使用类数组对象)对象创建一个新的浅拷贝的数组实例。

```js
console.log(Array.from('helloworld'))           //  ['h', 'e', 'l', 'l', 'o', 'w', 'o', 'r', 'l', 'd']
console.log(Array.from([1,3,6,9], x => x + x))  //  [2, 6, 12, 18]

// 关联对象 必须写length:  写多长，它截取多长 
// 索引对象
let obj = {
    0:'aa',
    1:'bb',
    2:'cc',
    'length':3,
}
const arr = Array.from(obj)
console.log(arr)  // ['aa', 'bb', 'cc']
```

6. some

> some：返回布尔值，判断是否有元素符合Func条件（有一个满足条件就返回true）

```js
const arr = [1,2,3,4,5]
console.log(arr.some( x => x % 2 === 0))  // true
```

7. every

> every：返回布尔值，判断每个元素符合Func条件（全部满足条件才返回true）。

```js
const arr = [1,2,3,4,5]
console.log(arr.every( x => x > 4))  // false
```

8. reduce

> **`reduce()`** 方法对数组中的每个元素按序执行一个由您提供的 **reducer** 函数  最后将其结果汇总为单个返回值。(计算数组总和等)

```js
let arr = [1,2,3,4,6]

const sum = arr.reduce((acc,cur) => {
    return acc+cur
})
// 计算总和
console.log(sum)
```



 

### 8.新数据类型

> `Symbol类型`本质上是一种**唯一标识符**，可**用作对象的唯一属性名，这样其他人就不会改写或覆盖你设置的属性值。**

```js
// Symbol 的主要作用是创建对象的私有成员，即使两个值相同的 Symbol 对象也是不相等的，因此可以用它来避免命名冲突。
const _name = Symbol('name')
const _age = Symbol('age')
const _getAge = Symbol('getAge')

class Persion {
    constructor(name,age){
        this[_name] = name;
        this[_age]  = age;
    }
    // 私有方法
    [_getAge] () {
        console.log('aa')
    }
    getName() {
        return this[_name]
    }
}
const pers = new Persion('goer',18)

console.log(pers[_name])  // goer  私有属性又可以访问了 干
console.log(pers._name)   // undefined

// _name 和 _age 都是 Symbol 类型的私有属性名，[_getAge] 是 Symbol 类型的私有方法名。这种写法可以避免属性和方法被外部对象访问到，保证了类的封装性。
```

> **Map** 是一组键值对的结构，和 JSON 对象类似。

```js
// map 基本数据结构
let map = new Map([['name','goer'],['passswd',"admin..."]])

// key 和 value 不仅可以是字符也可以是对象
let obj = {love:'ball',love2:'runing'}
map.set('love',obj)
console.log(map)
```

> map的:`curd`

```js
//初始化`Map`需要一个二维数组(请看 Map 数据结构)，或者直接初始化一个空`Map`
let map1 = new Map()

// 设置 key value
map1.set('name','管理员')

// 判断是否存在key  存在 true  否则false
map1.has('name')

// 根据 key 获取 value
map1.get('name')

// 删除
map1.delete('name')
```

> **set** 对象类似于数组，且成员的值都是唯一的

```js
// 数组去重 性能很好
let arr = [1,4,5,1,2,4]
let arr1 = [...new Set(arr)]
console.log(arr1)

// set 的curd 
let set = new Set([1,2])
set.add(2)
set.delete(1)
set.has(2)
console.log(set)
```





### 9. 导入和导出

> 模块导入和导出

```js
// 不支持模块化 添加type属性
<script src="./a.js" type="module"></script>
```

> 单个

```js
let love = ['ball','run']
//导出单个
export default love

// 引入单个
import love from "./data.js"
```

> 多个**常用**

```js
let name = 'goer'
let age  = 10

// 导出多个
export {name,age}
export const getDubble = (...props) => props.map(prop => prop * prop)

// 引入多个 as 修改名称 MyName
import { name as MyName,age } from "./data.js"
```



#### 10 Proxy

> 代理对象. **Proxy** 对象用于创建一个对象的代理，从而实现基本操作的拦截和自定义（如属性查找、赋值、枚举、函数调用等）。
>
> [mdn](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy)

```js
// 试想下 vue3是如何，修改数据，就会渲染到页面的 Proxy

// Proxy 对象用于创建一个对象的代理
const obj = { name: "goer", age:10, love:"test" }
const box = document.querySelector('.box')

// 修改内容  页面没有发生改变
box.textContent = obj.name

const prox1 = new Proxy(obj,{
    // 很多回调api

    //get 获取时候调用
    get(target,property){
        return obj[property]
    },

    // set 设置时候调用
    set(target,property,value){
        obj[property] = value
        box.textContent = obj.name
    }

})

prox1.name = "goer sex"
prox1.name = "chage goer"
```










