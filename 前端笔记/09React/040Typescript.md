---
title: 040Typescript
description: 
published: 1
date: 2023-06-09T10:15:11.676Z
tags: 
editor: markdown
dateCreated: 2023-05-11T10:53:56.344Z
---

<center>typeScript</center>



[toc]



## typeScript

> js的超集. `面向对象的方式`  是一种==新的编程思想，编程范式==



### 1.面向对象

> 面向过程： 分析出解决问题的步骤，然后逐步实现。 ==工作量大==
>
> 面向对象：找出解决问题的人，然后分配职责。

```tex
// 面向对象优点
(1) 思想层面：
-- 更接近于人的思维方式。
-- 有利于梳理归纳、分析解决问题。
(2) 技术层面：
-- 高复用：对重复的代码进行封装，提高开发效率。
-- 高扩展：增加新的功能，不修改以前的代码。
-- 高维护：代码可读性好，逻辑清晰，结构规整。
```



### 1. 定义类

> `constructor`定义一个构造器方法；

```js
class Person{
    name: string
    age: number
    
    constructor(name:string,age:number){
        this.name = name
        this.age = age
    }
    sayName = ():vodi => {

        console.log(`我是: ${this.name}`)
    }
}

// 声明使用
const goer = new Person('goer',10)
console.log(goer.name)
goer.sayName()
```





### 2.封装(重点)

> 封装好数据，类外面不能直接修改属性

```tsx
// 对ts的属性权限进行设置  和  php里面的面向对象一模一样
1. 公共属性(public)   没写任何说明的就是公共属性和方法，任何地方都可以访问和使用
	    public name: string
2. 静态属性(static)    声明为static的属性或方法不再属于实例，而是属于类的属性 不需要声明直接可以使用
		class Tools{
            static PI:number = 3.14
        }
        console.log(Tools.PI)
3. 只读属性(readonly)   属性便成了只读属性无法修改
		readonly age: number
```

> ts三种修饰符  和php一致

- `public`（默认值），可以在类、子类和对象中修改。 外部也可访问修改
- `protected` ，可以在类、子类中修改。外部不能访问
- `private` ，可以在类中修改。 外部不能访问，子类不能继承

```js
// public 没有特殊说明的都是public
class Person{
    public name: string
    public age: number

    public constructor(name:string,age:number){
        this.name = name
        this.age = age
    }
    public sayName = () => {

        console.log(`我是: ${this.name}`)
    }
}
```

```js
// protected 保护属性，可以在类中和子类中修改
class Person{
    public name: string
    protected age: number

    public constructor(name:string,age:number){
        this.name = name
        this.age = age
    }

    // 类里面修改保护属性
    public setAge = (chage:number):number => {
        this.age = chage
        return this.age
    }
}
const goer = new Person('goer',10)
goer.setAge(100)
console.log(goer)
```

```js
// private 私有属性 只能在该类里面访问使用
class Person{
    public name: string
    private age: number

    public constructor(name:string,age:number){
        this.name = name
        this.age = age
    }

    private sayHello = () => {
        console.log('hello')
    }
}

class GoodPerson extends Person{
    constructor(name:string,age:number){
        super(name,age)
    }

    public sayAge = () => {
        // age 只有是父类的私有属性不能继承的，报错了
        console.log(this.age)
    }
}
const goodGoer = new GoodPerson('good',10)
console.log(goodGoer)

```



### 3.继承

> 子类可以继承`extends` 父类的所有属性和方法
>
> 子类不调用`super`将会报错！

```tsx
class GoodPerson extends Person{

    love:string

    // 子类不调用`super`将会报错！
    constructor(name:string,age:number,love:string){
        super(name,age)
        this.love = love
    }
}
```





### 4.多态

> 向上兼容
>
> 子类中的方法会替换掉父类中的同名方法，这就称为方法的==重写==

```tsx
class Animal{
    private age
    constructor(public name: string,public sex: string){
        this.name = name
        this.sex = sex
    }

    setAge(numAge){
        // 设置私有属性
        this.age = numAge
        console.log(this.age)
    }
    // void 没有返回值
    shout():void{
        console.log('动物叫')
    }
    
}

// 继承 获取父类所有属性和 方法
class Dog extends Animal{

    // 多态： 重写父类方法
    shout():number{
        console.log('狗叫');
        return 1
    }
}

class Bind extends Animal{
    shout(): void {
        console.log('鸟叫')
    }
}

// 实例化 对象
let witheDog:Dog = new Dog('小白','男')

```



### 5. 抽象类

> 抽象类是专门用来被其他类所继承的类，它只能被其他类所继承不能用来创建实例

```tsx
abstract class Person{

    abstract run():void

    public sayName = () => {
        console.log('人在叫!!')
    }
}
class GoodPerson extends Person{

    run(): void {
        console.log("好人在跑")
    }
}

// 抽象类不能声明
// const goer = new Person()
const goer = new GoodPerson()
goer.sayName()
goer.run()
```

> `abstract`开头的方法叫做`抽象方法`，抽象方法`没有方法体只能定义在抽象类中，继承抽象类时抽象方法必须要实现`;



### 6. 接口

> 接口的作用类似于抽象类，不同点在于：接口中的所有方法和属性都是没有实值的，换句话说`接口中的所有方法都是抽象方法`；

> 接口主要负责定义一个类的结构，接口可以去`限制一个对象的接口`：对象只有包含接口中定义的所有属性和方法时才能匹配接口；

```tsx
// 检查接口类型
// 接口主要负责定义一个类的结构
interface Person{
    name:string
    sayHello():void
}

// 检查对象类型
class Student implements Person{

    public name:string
    constructor(name:string){
        this.name = name
    }
    sayHello(): void {
        console.log(`大家好，我是${this.name}`)
    }
}
const stu = new Student('goer')
stu.sayHello()
```



### 7. 泛型

> 定义一个函数或类时，有些情况下无法确定其中要使用的具体类型（返回值、参数、属性的类型不能确定）；
>
> 此时泛型便能够发挥作用；

```js
// 都用any是很不合适的，any会关闭TS的类型检查
const test = (arg:any):any => {
    return arg
}
```

> **创建泛型函数**  

```js
function test<T>(arg: T): T{
    return arg;
}
```

> T是我们给这个类型起的名字（不一定非叫T），设置泛型后即可在函数中使用T来表示该类型；
>
> 所以泛型其实很好理解，就表示某个类型；

```js
// 使用
1. 直接使用
test(10)
2. 指定类型
test<number>(10)
```

* 多个泛型

```js
function sayName<T,K>(a:T,b:K): K{
    return b
}

sayName<number,string>(10,"yes")

```

* 泛型类

```js
class MyClass<T>{
    props: T
    constructor(props:T){
        this.props = props
    }
    sayProps<T>():void{
        console.log(this.props)
    }
}
```

* 泛型继承

> 对泛型范围进行约束

```js
interface MyInter{
    length: number;
  }
  
function test<T extends MyInter>(arg: T): number{
    return arg.length;
}
```













<center>typescript</center>



[toc]





## typescript基础

>ts 目前大部分的组件都是ts所写，要学会呀
>
>> js也不能忘：==始于js，终于js==



### 1. 安装

```tsx
npm install typescript -g // 全局安装
```



### 2. 类型

> ：Boolean、Number、String、`null`、`undefined` 以及 ES6 的 [Symbol](http://es6.ruanyifeng.com/#docs/symbol) 和 ES10 的 [BigInt](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/BigInt)。

```js
1. string 
let str: string = "abc"

2. number
let num: number = 12 /NAN/Infinity无穷大

3. bool
let b: boolean = false

4. 空值
let u:void = undefined 
let uu:void = undefined
let nn:void = null
```



### 3. 任意类型

> tips :node 运行ts文件

```shell
// nodejs 环境执行ts
npm i @types/node --save-dev （node环境支持的依赖必装）
npm i ts-node --g
```

> 顶级类型

```js
1. any unknow  任意类型

let anys:any/unknow = 'aaa'
anys = true
console.log(anys);

//unknown可赋值对象只有unknown 和 any
let bbb:unknown = '123'
let aaa:any= '456'
 
aaa = bbb
```

> unknown 比 any 安全



### 3. 接口和对象类型

> **interface**来定义一种约束，让数据的结构满足约束的格式 
>
> > 接口可以继承

```js
// 两个 接口名称一样会 合并
interface Hi{
    name:string
    age : number,
    b?:string,
}
interface Hi{
    time:number 
}
let hiWorld:Hi = {
    name:'world',
    age: 12,
    time: 213213
}

console.log(hiWorld) //  { name: 'world', age: 12, time: 213213 }
    
>   b?:string, 可选操作符，可选择填写
```

```js
// 声明时添加任意属性
interface Hi{
    name:string,
    // 关键字 ： 类型
    [propName: string] :any
}
let hiWorld:Hi = {
    name:'world',
    abc : true,
    ggg : 'ggggllll'
}

console.log(hiWorld)
```

```js
// 联合类型 |  
let str: string | number = 'abc'

str = 111
console.log(str);  // 可以是两个类型
```

```js
// 只读属性 readonly
interface Hi{
    readonly name:string,
    // 关键字 ： 类型
    [propName: string] :any
}
name 只能读，不能修改
```

```js
// 添加方法
interface Person{
    name:string,
    age : number
    [propName: string] : any,
    // 定义函数
    eat():string;
}

let p:Person = {
    name:'小何',
    age: 11,
    add: ['1','2'],
    eat: () => {
        return '我吃了'
    }
}
console.log(p);
```



### 4.数组类型

> 数组

```js
// 数字类型的数组
let arr: number[] = [1,2,3]

let strArr: string[] = ['1','2']
// 任意类型
let anyArr: any[] = [true,'abc',1]

> 泛型声明
let arr:Array<number> = [1,2,2,2]

let strArr:Array<string> = ['a','b']

let anyArr:Array<any> = ['a',true,1,['bb'],{name:'bb'}]
```

```js
// 多维
let arr:number[][][] = [[[1,2],[3,4]]]

let arrTwo:Array<Array<Array<number>>> = [[[1,2],[3,4]]] 
一样的
```

```js
// 传递参数  arguments类数组
function Arr(...args:any):void{
    console.log(arguments)
    // 获取到所有参数   //ts内置对象IArguments 定义 
    let arr:IArguments = arguments
    console.log(arr[0]);
    
}
Arr(1,8,10)
```

> 一般通过==接口描述数组==

```tsx
interface anyArr{
    [index:number] :any
}

let anyAr:anyArr = ['a',1,{name:'bb'}]

console.log(anyAr[2].name)
```



### 5. 函数类型

> 函数类型

```tsx
// ?:可选操作符
const getFun = (name:string, age?:number):string => {
    return name+age
}

let get = getFun('goer')
console.log(get);
```

>接口约束函数值

```js

interface User{
    name:string
    age:number
}
// 参数和返回值都是 对象
const getFun = (user:User):User => {
    return user
}

let get = getFun({
    name:'小仔',
    age:77
})
console.log(get)
```

> 函数重载:  重载是方法名字相同，而参数不同，返回类型可以相同也可以不同。

```js

// 重载函数
function fun1(params:number):void

function fun1(params:string,params2:number):void

// 操作函数
function fun1(params:any , params2?:any):void{
    
    console.log(params);
    console.log(params2);
    
}
fun1(1)         // 执行第一个
fun1('ab',222)  // 执行第二个
```



### 6.联合类型和交叉类型

> 联合类型  | 
>
> 交叉类型 & 

```js
const fn = (something:number | boolean):boolean => {
     return !!something
}
// 不管参数是number 和 boolean 都会 !!0 转为 布尔值  (取反，再取反)
```
