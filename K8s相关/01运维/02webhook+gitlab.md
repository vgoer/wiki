<center>Webhook+gitlab</center>







[toc]







## webhook+gitlab

> git+hook: [blog](https://cloud.tencent.com/developer/article/2207775)





```shell
#!/bin/bash
echo ""
#输出当前时间
date --date='0 days ago' "+%Y-%m-%d %H:%M:%S"
echo "Start"
#git项目路径
gitPath="/www/wwwroot/api.baotongdzbhcom"
#git 网址
gitHttp="http://zouhu:456258Yas83@8.218.131.9:8099/vgoer/farmer_guarantee_api.git"
 
echo "Web站点路径：$gitPath"
 
#判断项目路径是否存在
if [ -d "$gitPath" ]; then
  echo "项目路径存在"
  cd $gitPath
  #判断是否存在git目录
  if [ ! -d ".git" ]; then
    echo "在该目录下克隆 git"
    cd ..
    # git clone --progress -v "http://zouhu:456258Yas83@8.218.131.9:8099/vgoer/farmer_guarantee_api.git" "/www/wwwroot/api"
    git clone --progress -v $gitHttp $gitPath
    # mv gittemp/.git .
    # rm -rf gittemp 
    echo "End"
  else  
    echo "在该目录下拉取 git"
    # git pull 2>&1
    git reset --hard
    git pull
    echo "End"
  fi
  exit
else
  echo "该项目路径不存在"
  echo "End"
  exit
fi
```

