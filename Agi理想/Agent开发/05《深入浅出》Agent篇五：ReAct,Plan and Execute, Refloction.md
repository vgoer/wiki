<center>05《深入浅出》Agent篇五：ReAct,Plan and Execute, Reflection</center>

[toc]





## ReAct,Plan and Execute, Reflection是什么

Agent 思考模式：

> ReAct: Reasoning and ect:  边规划边执行
>
> Plan and Execute:  规划后执行
>
> Reflection:   反思



### 1. ReAct

> 边规划，边执行
>
> 走一步看一步
>
> 适合： 小任务，通用型Agent, 而且常用。

![image-20261028143710329](05《深入浅出》Agent篇五：ReAct,Plan and Execute, Refloction.assets\image-20261028143710329.png)

> 缺点：
>
> 1. 可能进入死循环
> 2. 缺乏全局意识，缺乏规划



### 2. Plan and Execute

> 先规划，后执行
>
> 规划的LLM, 执行的LLM。 
>
> 适合 大任务。

![image-20261028151318191](05《深入浅出》Agent篇五：ReAct,Plan and Execute, Refloction.assets\image-20261028151318191.png)



### 3. Reflection

> 端对端
>
> 可以放置到 1 和 2中使用这个反思的llm

![image-20261028151728340](05《深入浅出》Agent篇五：ReAct,Plan and Execute, Refloction.assets\image-20261028151728340.png)



### 4. 适合的任务

![image-20261028152121776](05《深入浅出》Agent篇五：ReAct,Plan and Execute, Refloction.assets\image-20261028152121776.png)
