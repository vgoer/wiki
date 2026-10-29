<center>02《深入浅出》Agent篇二：RAG是什么</center>

[toc]





## RAG是什么

> (Retrieval Augmented Generation)
>
> ​    召回           增强            生成

```shell
LLM 生成能力  增强能力  召回能力

1. 训练之后的数据
2. 公司内部的文档
3. 自己的笔记
```



> Retrieval 召回

![image-20261028112208941](02《深入浅出》Agent篇二：RAG是什么.assets\image-20261028112208941.png)

```shell
文档总量特别大，不能直接给LLM
   召回文档（少，准）

业界主要的方式就是： 拆成 chuok 数据库

查询方式：
    1. 关键字模糊查询
    2. 语义搜索    => Embedding  => 拆成向量： 匹配向量是否相近来看语义是否相同
```



> Augmented 增强
>
> 将用户的输入，文档，输出，重新给LLM训练。



以上出现一些核心问题？

```shell
1. 用户输入太乱
	解决： 新增一层LLM => 提取重点
	
2. 切割成chuok,文章连续内容被切割了。
	解决： 精简输入作为chuok
```

