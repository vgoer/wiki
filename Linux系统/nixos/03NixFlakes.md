<center>Nix Flakes</center>



[toc]





## Nix Flakes

> 1. 解决多个安装多个 channel,导致软件版本不同
> 2. 不能用一个文件管理所有配置文件
>
> `nix flakes`解决这两个问题

```shell
# 创建复制文件
mkdir -p my-nix-flake/nixos
mkdir -p my-nix-flake/home-manager
cp /etc/nixos/configuration.nix my-nix-flake/nixos
cp /etc/nixos/hardware-configuration.nix my-nix-flake/nixos
cp ~/.config/home-manager/home.nix my-nix-flake/home-manager
cp -r ~/.config/home-manager/apps/ my-nix-flake/home-manager
```

> 添加文件``my-nix-flake/flake.nix`.`

```shell
{
  description = "Your new nix config";

  inputs = {
    nixpkgs.url = "github:nixos/nixpkgs/nixos-unstable";
    home-manager = {
      url = github:nix-community/home-manager;
      inputs.nixpkgs.follows = "nixpkgs";
    };
  };

  outputs = { nixpkgs, home-manager,  ... }:
  let
    system = "x86_64-linux";
  in
  {
    nixosConfigurations = {
      <your_host_name> = nixpkgs.lib.nixosSystem {
        inherit system;
        modules = [
          ./nixos/configuration.nix
          home-manager.nixosModules.home-manager
          {
            home-manager = {
              useUserPackages = true;
              useGlobalPkgs = true;
              users.<your_user_name> = ./home-manager/home.nix;
            };
          }
        ];
      };
    };
  };
}

```

> 修改`etc/nixos/configuration.nix` 添加 ` "nix-command" "flakes"`

```shell
sudo vim /etc/nixos/configuration.nix

nix.settings.experimental-features = [ "nix-command" "flakes" ];

cp /etc/nixos/configuration.nix my-nix-flake/nixos
```

> 开始安装
>
> Then we can run `sudo nixos-rebuild switch --flake './my-nix-flake#<your_host_name>'` to apply system wide configuration changes and `home-manager switch --flake ./my-nix-flake#<USERNAME>` to apply home manager changes.

```shell
sudo nixos-rebuild switch --flake './my-nix-flake#<your_host_name>'

# 进入配置文件  一定要进入我们创建的配置文件， 一定不要少了 . 代表当前目录
home-manager switch --flake .#goer  # 少用因为下面这个命令就可以管理我们所有配置文件了
sudo nixos-rebuild switch --flake '.#nixos'

```

