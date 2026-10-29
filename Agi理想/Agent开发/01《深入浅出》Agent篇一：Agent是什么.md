<center>01《深入浅出》Agent篇一：Agent是什么</center>

[toc]





## Agent是什么

> 豆包？ Deepsee? chatGPT? Claude?
>
> 这些是在LLM上封装了很多功能的 Agent的么？
>
> Agent是什么：包括LLM和一系列以LLM为核心的补丁。

```shell
LLM (Large Language Model)
大模型
无状态的机器 y=f(x)   => llm是没有记忆的
论文：Attention is all you need (注意力机制)  => 大力出奇迹的东西
原理：预测下一个Token  => 根据用户输入，预测下输出，这样轮回。
```

> LLM的问题：
>
> 1. 没有记忆
> 2. 有时效性，数据不完备
> 3. 输出单一
> 4. 基于概率预测，天然有不准确或者幻觉缺陷

> 解决问题：

![image-20261028104614566](01《深入浅出》Agent篇一：Agent是什么.assets\image-20261028104614566.png)



举例：

> 博学 审问 慎思 明辨 笃行

![image-20261028110704541](01《深入浅出》Agent篇一：Agent是什么.assets\image-20261028110704541.png)