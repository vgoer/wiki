<center>Katana爬虫框架</center>







[toc]









## Katana爬虫框架

> 下一代爬行和蜘蛛框架。
>
> [github](https://github.com/projectdiscovery/katana)











### 1. 安装

```shell
# 安装
go install github.com/projectdiscovery/katana/cmd/katana@latest
# 复制到环境变量中 
sudo cp ~/go/bin/katana /usr/local/bin

katana -u https://adjvendor.paypal.com -o urls.txt

# 使用 
katana -h
```







### 2.使用

```shell
# 基本参数
# 基本使用
katana -u https://example.com

# 指定爬取深度 (默认为 2)
katana -u https://example.com -d 3

# 并发数控制 (默认为 10)
katana -u https://example.com -c 20

# 指定输出文件
katana -u https://example.com -o output.txt

# 高级参数
# 爬取范围控制
-hs, --headless          # 使用 headless chrome 进行爬取
-jc, --js-crawl         # 启用 JavaScript 爬取
-aff, --automatic-form-fill  # 自动填充表单
-fs, --field-scope      # 字段作用域

# 爬取策略
-nc, --no-colors        # 禁用彩色输出
-silent                 # 静默模式
-timeout int           # 请求超时时间 (默认 10 秒)
-rd, --root-domain     # 仅爬取根域名
-sf, --scope-filter    # 使用自定义范围过滤

# 输出格式控制
-f, --field string     # 指定输出字段 (url/path/fqdn/rdn/qurl/qpath等)
-j, --json            # JSON 输出

# 组合使用
# 1. 完整扫描示例
katana -u https://example.com \
  -d 3 \
  -c 20 \
  -jc \
  -hs \
  -o results.txt

# 2. 仅获取 URL 参数的扫描
katana -u https://example.com \
  -f qurl \
  -silent \
  -d 2

# 3. 配合其他工具的扫描
echo "https://example.com" | katana -silent -f url | httpx -silent

# 4. 使用代理扫描
katana -u https://example.com \
  -proxy http://127.0.0.1:8080

# 5. 带有延迟的扫描
katana -u https://example.com \
  -delay 2 \
  -random-delay
  
  
# 常用
# 用于漏洞扫描的组合
katana -u https://example.com \
  -d 3 \
  -c 15 \
  -f qurl \
  -jc \
  -silent \
  -timeout 20 \
  -o urls.txt

# 用于资产发现的组合
katana -u https://example.com \
  -d 2 \
  -rd \
  -f fqdn \
  -silent \
  -aff \
  -o domains.txt
```







### 3. 被动扫描

> 不主动扫描主机

1. `-fs` (--passive)    - 启用被动扫描模式 - 指定要使用的被动扫描数据源
3. `-f qurl`     - 指定输出格式为 `qurl`

````shell
# 被动搜索信息地方
- waybackarchive
- commoncrawl
- alienvault
- urlscan
- github
- virustotal

echo "https://google.com" | katana -fs waybackarchive,commoncrawl,alienvault,urlscan,github,virustotal -f qurl > google.txt
````

