<center>08《深入浅出》Agent篇八：Memory记忆</center>

[toc]





## Memory记忆

> LLM缺点： 没有记忆。
>
> 解决这个问题的两个流派：
>
> 1. 使用数据库
> 2. md 蒸馏。

![image-20261029133055489](08《深入浅出》Agent篇八：Memory记忆.assets\image-20261029133055489.png)



### 1. RAG

> 1. chunk 
>
> insert: 把一些重要的信息提取 chunk 插入到数据库（es)中。  es: 支持关键字和向量
>
> query: 两种方式：
>
> * 直取：  把用户的query embed成向量，直接去数据库比对，recall一堆记忆出来。
> * LLM取： LLM分析用户的query，生成更适合的方案。

![image-20261029134050086](08《深入浅出》Agent篇八：Memory记忆.assets\image-20261029134050086.png)

> 2. md
>
> Md (claude code使用的方法)   业界很多使用这种。

>  三级： 
>
> MEMORY.md： 偏好，简介
>
> Topics.md：约束
>
> Transcript.md: row message 元数据，保底

![image-20261029135405476](08《深入浅出》Agent篇八：Memory记忆.assets\image-20261029135405476.png)

