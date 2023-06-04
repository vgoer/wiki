<center>NixConfiguration</center>





[toc]





### NixConfiguration

> 下面我们介绍 `NixConfiguration`的配置文件

```shell
# hardware-configuration.nix 硬件的配置    configuration.nix 系统和软件的配置
configuration.nix

{
	imports = [
	  #硬件配置
	  ./hardware-configuration.nix
	];

   # boot的配置 grup  uefi 
   boot.loader.grub.enable = true;
   boot.loader.grub.efiSupport = true;
   boot.loader.grub.efiInstallAsRemovable = true;
   boot.loader.efi.efiSysMountPoint = "/boot";
   boot.loader.grub.device = "/dev/sda";
   
   # 镜像源
   nix.settings.substituters = lib.mkBefore [
    #"https://mirror.sjtu.edu.cn/nix-channels/store"
    "https://mirrors.tuna.tsinghua.edu.cn/nix-channels/store"
    #"https://mirrors.bfsu.edu.cn/nix-channels/store"
   ];
   
   # 时区
   time.timeZone = "Asia/Shanghai";
   
   # 本地化 ，语言
   i18n.defaultLocale = "zh_CN.UTF-8";
   # 终端
   console = {
     # font="Lat2-Terminus16";
	 font = "DroidSansMono";
	 keyMap = "us";
	 useXkbConfig = true;
     
   };
   # 输入法
   i18n.inputMethod.enabled = "ibus";
   i18n.inputMethod.ibus.engines = with pkgs.ibus-engines; [ libpinyin ]; 
   
   # 桌面环境
   # enable the x11 windows system
   services.xserver.enable = true;
   # 登录管理器 gdm
   services.xserver.displayManager.gdm.enable = true;
   # gnome桌面环境
   services.xserver.desktopManager.gnome.enable = true;
   # 桌面共享
   services.gvfs.enable = true;
   
   # 开启声音
   # Enable sound.
   dound.enable = true;
   hardware.pulseaudio.enable = true;
   
   # 设备输入
   services.xserver.libinput.enable = true;
   
   
   # 用户的配置 重要 goer是我的用户名，替换为你自己的
   users.users.goer={
     # 正常用户
     isNormalUser = true;
     # 该用户使用的shell
     shell = pkgs.zsh；
     # 该用户的组
     extraGroups = ["wheel" "docker"]；
     # ssh key登录
     openssh.authorizedKeys.keys = [
       "key1";
       "key2";
     ];
     
     # 该用户的软件
     packages = with pkgs;[
     	bat
     ];
   };
   #默认开启zsh
   programs.zsh.enable = true;
   environment.shells = with pkgs; [zsh];
   
   # 系统环境的软件
   environment.systemPackages = with pkgs;[
   	 vim
   	 wget
   	 curl
   	 sl
   	 neofetch
   	 btop
   	 htop
   ];

   
   # 开启ssh服务配置
   # Enable the openssh 
   services.openssh.enable = true;
   # 禁止使用密码登录
   services.openssh.settings.PasswordAuthentication = false;
   # 允许root使用密码登录 不要开
   # services.openssh.permitRootLogin = "yes";
   
   
   
   # 安装docker
   virtualisation.docker.enable = true;
   
   # nix flakes命令打开
   nix.settings.experimental-features = [ "nix-command" "flakes" ];
 
   # 系统 版本
   system.stateVersion = "23.05";
}




```

> nix清理缓存，删除没用的文件

```shell
sudo nix-collect-garbage -d
```

