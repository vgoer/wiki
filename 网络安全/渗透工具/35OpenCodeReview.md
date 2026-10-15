<center>OpenCodeReview</center>





[toc]





## OpenCodeReview

> Open Code Review 是一款基于人工智能的代码审查命令行工具。它最初是阿里巴巴集团内部的官方人工智能代码审查助手，在过去的两年里，它服务了数万名开发者，并识别出了数百万个代码缺陷。经过大规模的充分验证，我们将其孵化成一个开源项目，供社区使用。只需配置一个模型端点即可开始使用。
>
> [github](https://github.com/alibaba/open-code-review)





### 1. 安装

```shell
npm install -g @alibaba-group/open-code-review

# 快速入门 配置llm
ocr config provider          # Select a built-in provider or add a custom one
ocr config model             # Pick a model for the active provider
```





### 2. 使用

> [office](https://open-codereview.ai/)

```shell
cd your-project

# 工作区模式 — 审查所有已暂存、未暂存及未追踪的变更
ocr review

# 分支范围模式 — 审查功能分支自脱离主分支（main）以来的变更（基于合并基点）
ocr review --from main --to feature-branch

# 单个提交模式
ocr review --commit abc123

# 恢复中断的范围或提交审查
ocr session list
ocr review --from main --to feature-branch --resume <session-id>

# 全文件扫描模式 — 审查整个文件而非仅差异内容（无需 Git 历史记录）
ocr scan                          # 扫描整个仓库
ocr scan --path internal/agent    # 扫描特定目录或文件
ocr scan --resume <session-id>   # 恢复中断的全文件扫描

# 将结果保存到文件（推荐用于 AI 宿主代理）
ocr review --format json --output result.json

# 委托模式 — 让 AI 编码代理自行执行审查
# OCR 负责文件选择和规则解析；无需配置 LLM
ocr delegate preview
ocr delegate rule src/main.go src/handler.go
```

