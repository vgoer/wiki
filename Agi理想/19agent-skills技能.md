<center>agent-skills技能</center>



[toc]





## agent-skills技能

> 具备人工智能编码代理的生产级工程技能。 [github](https://github.com/addyosmani/agent-skills)
>
> [doc](https://skills.addy.ie/)



### 1. 安装

```shell
npx skills add addyosmani/agent-skills            # install all 25 skills
npx skills add addyosmani/agent-skills --list     # browse before installing
```

```shell
  DEFINE          PLAN           BUILD          VERIFY         REVIEW          SHIP
 ┌──────┐      ┌──────┐      ┌──────┐      ┌──────┐      ┌──────┐      ┌──────┐
 │ Idea │ ───▶ │ Spec │ ───▶ │ Code │ ───▶ │ Test │ ───▶ │  QA  │ ───▶ │  Go  │
 │Refine│      │  PRD │      │ Impl │      │Debug │      │ Gate │      │ Live │
 └──────┘      └──────┘      └──────┘      └──────┘      └──────┘      └──────┘
  /spec          /plan          /build        /test         /review       /ship
```





### 2. 使用

> 使用教程 [doc](https://skills.addy.ie/tutorials/new-app/)

```shell
1. spec

eg: /spce  我想要开发一款xxx的应用，（具体的需求）。请一次问一个问题，知道你对我真实的需求约有98%的把握，然后停止提问。提问内容包含：（应用具体的一些需求）

eg: 
 我想开发一个记录跑步的app。请一次问我一个问题，知道你对我的真实需求约有98%的把握后，停止提问。提示内容包含：涉及那些习惯，频率，连续打卡，数据保存
 
 遵循规范驱动开发范式，使用spec-driven-development技能编写一份 SPEC.md文件。文件内容包含：项目目标，localStorage数据模型，核心页面与状态，连续打卡规则，测试策略以及明确的非目标。
 
 根据spec生成一个可读性好的hmtl文件。

 （根据生成的文档，确认具体的需求。然后才能开始代码编写）
```

```shell
2. plan

eg: 
使用planning-and-task-breakdown这个skill，将 SPEC.md 的内容转化为一个个小而可验证的任务，并明确验收标准与依赖顺序：添加习惯、标记某日完成、计算连续打卡天数、展示统计数据、持久化存储至 localStorage。将这些任务记录在 tasks/plan.md 文件中。
```

```shell
3. build

eg: 
使用 incremental-implementation 和 test-driven-development。 着手实现第一个任务，xxx,先编写会失败的测试用例，涵盖各个边界情况，，确保测试通过，然后提交代码。


/build auto  一次性完成整个计划。你只需批准一次计划，它就会自动执行每个任务，仍然采用测试驱动开发，并且每个任务都会提交，仅在失败或遇到风险步骤时暂停。

```

```shell
4. ui

eg:
 使用 api-and-interface-design 。涉及xxx个模块的的用户界面
```

```shell
5. test

eg: 先配置 Chrome DevTools MCP
使用 browser-testing-with-devtools，运行完整的测试套件。随后在真实浏览器中进行验证： xxx 功能，
```

```shell
6. review

eg:
遵循涵盖所有五个维度的代码审查与质量标准，随后进行侧重于安全与加固的检查：确保安全地渲染用户输入的习惯名称（防止 XSS 攻击），并验证从加载的数据——因为这些数据属于不可信来源，可能遭到篡改或损坏。
```

```shell
7. ship

eg: 
请遵循发布与上线流程: 制定上线前检查清单，将新功能（作为一种新行为模式）置于功能开关（feature flag）控制之下，规划部署至任意静态托管平台，并编写回滚步骤。最后，请给出“执行”或“暂缓”的明确
```





### 2. 代码库

> 你可以自带代码库，或者克隆任何你喜欢的开源应用。

```shell
1. 上下文工程  

eg: 重要
使用context-engineering的skill，阅读该仓库，并编写一份简短的规则文件 (CLAUDE.md或AGENTS.md)， 总结其中的各项惯例：包括项目结构、代码模式、测试命令，以及新手容易出错的地方。
```

```shell
2. 基于源代码开发

eg: 
使用source-driven-development，若需在此技术栈中实现速率限制，请对照所用框架及库的官方文档核实方案，注明资料来源，并标记出任何无法确认的内容。
```

```shell
3. /spec 规范驱动开发

eg:
使用 spec-driven-development， 但将范围限定为对现有系统内部的某项变更。针对单个 API 端点制定限流规范，内容应涵盖：接入点、配置、限流阈值、错误响应，以及必须保持不变的部分。
```

```shell
4. 怀疑驱动开发

eg: 务必对计划抱有怀疑态度
使用 doubt-driven-development对方法应用与该计划。在全新的视角下，提取出关于现有代码行为的每一项非显而易见的断言，对照实际源码逐一尝试反驳，并最终达成一致。
```

```shell
5. build 增量式实施

eg:
使用 incremental-implementation and test-driven-development，遵循增量实现与测试驱动开发（TDD）的原则。首先添加特征测试（characterization tests）以锁定端点的当前行为，随后在功能开关（feature flag）的控制下，以小而完整的增量方式实现xx功能。
```

```shell
6. 调试与错误恢复

eg:
修改后，某项测试失败了。请遵循 debugging-and-error-recovery: reproduce 调试与错误恢复的流程，复现问题，定位问题，捡起简化为最小的失败用例，修复根本原因，并添加房屋措施以防止问题复发。
```

```shell
7.  review 代码审查和质量

eg:
使用  code-review-and-quality。 遵循代码审查与质量标准：将变更规模控制在约 100 行以内，确保其符合既有模式，并针对新增的限制条件及错误处理路径进行安全与加固检查。
```

```shell
8. ship 发布

eg:
使用  shipping-and-launch 。 遵循发布与上线流程，利用功能开关（feature flag）规划分阶段发布，针对速率限制器配置必要的监控，并制定回滚方案。最后，请给出执行或暂缓的决定。
```







### 3. 循环工程

> 本教程将构建一个最小的、诚实的软件工厂：一个循环，它会在你睡觉时改进你的代码库，并在早上请求你签字确认。

```shell
1. 代码简化

eg:
code-simplification 请帮我选定一个可以自动化的改进点，要求其范围狭窄、高频发生且难以伪造：例如，每次运行修复一个 Lint 违规问题，或者移除一个不必要的可选属性（prop）。该改进必须能够通过 通过/失败 （绿/红）的检查来验证。
```



