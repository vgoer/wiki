<center>HomeManager</center>





[toc]







### HomeManager

> 上一步安装软件我们都是在 `/etc/nixos/configuration.nix`中，那我们每个软件的单独配置放哪里呢。
>
> 放这个文件会导致文件太乱。让每个软件的配置都可以像nixos配置一样保存下来呢。 这就是我们今天的主角 `HomeManager`





### 1. 第一种安装方式  

> `nixos Nix Module`方式 [github]()

```shell
# 添加 channel
# 系统是 23.05 

sudo nix-channel --add https://github.com/nix-community/home-manager/archive/release-23.05.tar.gz home-manager
sudo nix-channel --update
sudo nix-channel --update --verbose
# 修改配置文件 /etc/nixos/configuration.nix
```

- Add `<home-manager/nixos>` under ***imports***
- Set `users.users.<name>.isNormalUser = true;` (in my case this is already **true**)
- Add `home-manager.users.<name> = { pkgs, ... }: {` related config as below

```shell
 # 1. 
 imports = [
 	<home-manager/nixos>
 ]
 # 2.
users.users.goer=[
	isNormalUser = true
]
 
 # 3. 
 home-manager.users.<name> = { pkgs, ... }: {
     home.packages = [ pkgs.atool pkgs.httpie ];
     home.stateVersion = "23.05";
 };
 
 # 安装
sudo nixos-rebuild  switch

# 使用
httpie --version
```



#### a. 使用

> 分离系统的配置和个人用户软件的配置

```shell
# 新建home.nix文件
sudo vim /etc/nixos/home.nix

{ config, pkgs, ... }:

{
  home.username = "<username>";
  home.homeDirectory = "/home/<username>";
  home.stateVersion = "23.05";
}
```

> 修改`homemanager`在`/etc/nixos/configuration.nix`里面的配置

```shell
# 修改配置
  home-manager = {
    useGlobalPkgs = true;
    useUserPackages = true;
    users.<name> = import ./home.nix;
    home.packages = [ pkgs.atool pkgs.httpie ];
  };
  
# 安装
sudo nixos-rebuild  switch
```







### 2. 第二种方式

> `homeManager`自己管理软件包。推荐使用这种 [安装配置](https://nix-community.github.io/home-manager/)

```shell
# 删除使用nix管理的homeManager
sudo nix-channel --add https://github.com/nix-community/home-manager/archive/release-23.05.tar.gz home-manager
sudo nix-channel --update
sudo nix-channel --update --verbose
# 修改配置文件 /etc/nixos/configuration.nix
```

> 安装

```shell
nix-shell '<home-manager>' -A install

# 使用
home-manager
```

> 和第一种的对比

Config folders (where we can find `home.nix`):

- NixOS module: `/etc/nixos`
- Standalone: `~/.config/home-manager`

Update nix channel:

- NixOS: `sudo nix-channel --update`
- Home manager: `nix-channel --update`

Apply home manager changes command:

- NixOS module: `sudo nixos-rebuild switch`
- Standalone: `home-manager switch`



#### a. 使用

```shell
# 安装后默认创建了home.nix文件
vim ~/.config/home-manager/home.nix  

# 安装软件
  home.packages = with pkgs; [
    htop
  ];
  
# 哇哦，哇哦
htop
```

> 配置分离，向上面一样 `放到apps目录下`

#### zsh

```shell
#  zsh配置
vim ~/.config/home-manager/apps/zsh.nix

{
  programs.zsh = {
    enable = true;
    enableCompletion = true;
    enableAutosuggestions = true;
    enableSyntaxHighlighting = true;
    oh-my-zsh = {
      enable = true;
      plugins = [ "docker-compose" "docker" ];
      theme = "dst";
    };
    initExtra = ''
      bindkey '^f' autosuggest-accept
    '';
  };

  programs.fzf = {
    enable = true;
    enableZshIntegration = true;
  };
}

# 配置home.nix文件 引入
vim ~/.config/home-manager/home.nix 

  imports = [
    ./apps/zsh.nix
  ];
```

> zsh配置默认shell

```shell
# 添加配置  /etc/nixos/configuration.nix

users.users.<name>.shell = pkgs.zsh;
environment.shells = with pkgs; [ zsh ];
# 报错提示 开启zsh 添加下一行
programs.zsh.enable = true;
```



#### micro

> 编辑器

```shell
# 
vim ~/.config/home-manager/apps/micro.nix

{
  programs.micro = {
    enable = true;
    settings = {
      colorscheme = "material-tc";
      mkparents = true;
      softwrap = true;
      tabmovement = true;
      tabsize = 2;
      tabstospaces = true;
      autosu = true;
    };
  };
}


# 配置home.nix文件 引入
vim ~/.config/home-manager/home.nix 

  imports = [
    ./apps/zsh.nix
    ./apps/micro.nix
  ];
```





