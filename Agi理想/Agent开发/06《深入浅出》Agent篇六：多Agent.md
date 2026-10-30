<center>06《深入浅出》Agent篇六：多Agent</center>

[toc]





## 多Agent

> Multi Agent 多Agent
>
> Single Agent 单Agent

![image-20261028153410788](06《深入浅出》Agent篇六：多Agent.assets\image-20261028153410788.png)

> supervision: Multi-Agent 中的 Supervisor（主管/监督者）

**权责清晰**：底层的每个 Agent 只需要专心做好自己的垂直领域（比如数学计算、文本润色、代码生成），不需要操心全局逻辑，提示词（Prompt）可以写得很纯粹。   

**安全性与可控性（Governance）**：所有的工具调用、跨 Agent 传话都经过 Supervisor 把关，能有效防止 Agent 产生“无限递归死循环”或越权操作。   

**主流框架原生支持**：像 LangChain 推出的 `langgraph-supervisor` 等工具，就是专门用来几行代码快速搭建这种“一个主管带几个精兵强将”的层级化多智能体系统的。   