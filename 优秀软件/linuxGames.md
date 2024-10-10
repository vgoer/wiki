<center>LinuxGames</center>





[toc]







## Linux Games

> linux上玩游戏，美滋滋
>
> [lol](https://leagueoflinux.gitlab.io/install/)
>
> Lutris[lutris](https://lutris.net/downloads)
>
> steam [steam](https://store.steampowered.com/)





### 1. install

```shell
sudo pacman -S lutris


sudo pacman -S steam


```



### 2. gamescope

> [wiki(https://apiclo.github.io/gamescope/)
>
> [bilibili](https://www.bilibili.com/video/BV1YYeyeZEtz/?spm_id_from=333.999.0.0&vd_source=86b829d6caeffc65037786a84ec2cb17)

```shell
paru -S gamescope-plus gamescope-session-plus gamescope-session-steam
# 如果使用的yay或者其它aur工具，请自行替换

# 安装steam-runtime
yay -S steam

# 作者的一件脚本
curl -sSL https://raw.githubusercontent.com/Apiclo/Apiclo.github.io/master/shells/steamos.sh -o steam-session.sh && /bin/bash steam-session.sh
```

