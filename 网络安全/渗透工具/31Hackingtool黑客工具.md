<center>Hacking Tool</center>



[toc]





## hacking tool

> 黑客必备的一体化黑客工具 [github](https://github.com/Z4nzu/hackingtool)



## 1. 渗透流程。

```shell
目标必须是你拥有、或拿到书面授权测试的(自有资产、公司授权范围、CTF/靶场、或 bug bounty 项目明确列入 scope 的域名)。在这个前提下,"找 bug" 可以拆成一条标准流水线,而这个仓库的 21 类 215 个工具几乎就是按这条流水线组织的。

总体思路:找 bug 不是上来就扫,而是 收敛攻击面 → 逐个面找弱点 → 验证可利用性 → 记录复现。阶段如下:

阶段 0:确定 scope,先被动、不碰目标
   - 只做被动 OSINT:whois、证书透明度、DNS 记录。工具类:Information gathering 下的
     theHarvester / dnsrecon / dnsenum / sublist3r 等(26 个工具)。
   - 目的:在动手前搞清楚"这个目标到底暴露了哪些资产",并确认哪些在授权范围内。
   - 这阶段零风险,也最能体现思路:很多 bug 来自"没人知道的子域/旧服务"。

阶段 1:资产测绘(子域 + 存活主机)
   - 子域枚举:subfinder、amass、dnsx — 扩出子域列表
   - 存活探测:httpx — 从一堆子域里筛出活着的 HTTP 服务(状态码、标题、技术栈)
   - 端口与服务:nmap / naabu — 对存活 IP 扫端口,找非标准端口上的服务
   - 产物:一份"目标攻击面清单",这是后面所有工作的索引

阶段 2:指纹识别,按图索骥
   - whatweb / wappalyzer 类指纹:知道每个站点是什么框架、什么版本
   - 版本一出来,bug 方向就清晰了:老框架 → 找已知 CVE 打补丁差;
     自研应用 → 找逻辑漏洞;CMS/开源组件 → 对照公开漏洞库
   - 这一步决定你接下来重点看什么,而不是无差别乱扫

阶段 3:内容发现 + 漏洞批量扫描
   - 目录/文件发现:ffuf / gobuster — 找隐藏路径、备份文件、.git 泄露、管理后台
   - 漏洞模板扫描:nuclei — 用现成模板批量打已知漏洞(快但会有误报,需人工复核)
   - Web 专项:xss_attack 类(dalfox 等)、sql_injection 类(sqlmap 等)针对参数测
   - 注意:这个仓库的 ddos / phishing / RAT / wifi_jamming 等类别对找 bug 没用甚至越界,
     bug bounty 里用不到,跳过即可

阶段 4:逻辑漏洞 — 工具装不了脑子,这是产出大头
   - 工具能帮你测注入/XSS/暴露面,但 IDOR(越权访问他人数据)、支付逻辑、
     状态机绕过、权限提升这些,只能靠你手工点:注册两个账号互相访问资源、
     改请求里的 ID/价格/状态字段、测接口的鉴权是否只在前端
   - 方法论参考仓库自带的 skill/METHODOLOGY.md 和 OPERATOR.md(/skill 可看)

阶段 5:记录与报告
   - 每个发现记下:URL、请求/响应、复现步骤、影响
   - headless 模式可辅助:
     hackingtool --engagement <目标> --targets <域名> --pipeline recon
     hackingtool --engagement <目标> --report
   - bug bounty 提交时,"清晰的复现步骤 + 真实影响"比"扫出一堆高危误报"值钱得多

在容器里实际操作时,工作流会是:
  docker compose run --rm hackingtool
  然后 @tag:recon 挑工具 → 1 安装 → 2 运行,或 /ai 描述需求让它推荐,
  /goal 让它规划并逐步确认执行。
```

