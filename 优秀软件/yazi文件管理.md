<center>yazi</center>









[toc]







## yazi

> 终端文件管理器。 [github](https://github.com/sxyazi/yazi)  [yazi](https://yazi-rs.github.io/)







### 1. 安装

> install

```shell
sudo pacman -S yazi ffmpegthumbnailer p7zip jq poppler fd ripgrep fzf zoxide imagemagick
```



> 它能够在退出 Yazi 时更改当前工作目录。

```shell
# .bashrc  .zhsrc

function y() {
	local tmp="$(mktemp -t "yazi-cwd.XXXXXX")"
	yazi "$@" --cwd-file="$tmp"
	if cwd="$(cat -- "$tmp")" && [ -n "$cwd" ] && [ "$cwd" != "$PWD" ]; then
		builtin cd -- "$cwd"
	fi
	rm -f -- "$tmp"
}
```

