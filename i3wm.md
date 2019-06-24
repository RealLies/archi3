# 设置全局中文(https://wiki.archlinux.org/index.php/Localization/Simplified_Chinese_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))

1. sudo vim /etc/locale.gen
```
en_US.UTF-8 UTF-8
zh_CN.UTF-8 UTF-8
```
2. locale-gen
3. sudo vim ~/.xinitrc或~/.xprofile单独设置中文locale(.xinitrc: 每次startx启动X界面时读取并运用里面的设置,.xprofile: 每次使用gdm等图形登录时读取并运用里面的设置)
```
export LANG=zh_CN.UTF-8
export LANGUAGE=zh_CN:en_US
export LC_CTYPE=en_US.UTF-8
```
4. yay -S wqy-microhei wqy-microhei-lite wqy-bitmapfont wqy-zenhei ttf-arphic-ukai ttf-arphic-uming adobe-source-han-sans-cn-fonts adobe-source-han-sans-cn-fonts noto-fonts-cjk
5. 修改或创建文件/etc/fonts/conf.avail/64-language-selector-prefer.conf
6. noto-fonts-cjk用户：
```html
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
  <alias>
    <family>sans-serif</family>
    <prefer>
      <family>Noto Sans CJK SC</family>
      <family>Noto Sans CJK TC</family>
      <family>Noto Sans CJK JP</family>
    </prefer>
  </alias>
  <!--以上为设置无衬线字体优先度-->
  <alias>
    <family>monospace</family>
    <prefer>
      <family>Noto Sans Mono CJK SC</family>
      <family>Noto Sans Mono CJK TC</family>
      <family>Noto Sans Mono CJK JP</family>
    </prefer>
  </alias>
  <!--以上为设置等宽字体优先度-->
</fontconfig>
```
6. adobe-source-han-sans-otc-fonts用户：
```html
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
  <alias>
    <family>sans-serif</family>
    <prefer>
      <family>Source Han Sans SC</family>
      <family>Source Han Sans TC</family>
      <family>Source Han Sans HW</family>
      <family>Source Han Sans K</family>
    </prefer>
  </alias>
  <!--以上为设置无衬线字体优先度-->
  <alias>
    <family>monospace</family>
    <prefer>
      <family>Source Han Sans SC</family>
      <family>Source Han Sans TC</family>
      <family>Source Han Sans HW</family>
      <family>Source Han Sans K</family>
    </prefer>
  </alias>
  <!--以上为设置等宽字体优先度-->
</fontconfig>
```
7. ln -s /etc/fonts/conf.avail/64-language-selector-prefer.conf /etc/fonts/conf.d/64-language-selector-prefer.conf
8. fc-cache -fv
9. fc-match -s |grep 'Noto Sans CJK'

# 设置中文输入法
1. pacman -S fcitx fcitx-im fcitx-configtool
2. pacman -S fcitx-sogoupinyin
3.  /etc/profile文件，在文件开头加入三行：
```
export XMODIFIERS="@im=fcitx"
export GTK_IM_MODULE="fcitx"
export QT_IM_MODULE="fcitx"
```

# chrome浏览器
yay -S chromium

# 安装rofi
1. yay -S rofi
2. 进入~/.config/i3/config
3. 在末尾添加
```
# 替代原始应用程序启动器(solarized_alternate为主题)
bindsym $mod+d exec rofi -show drun -theme solarized_alternate
```
4.注释掉这一行
```
# bindsym $mod+d exec rofi -show drun -theme solarized_alternate
```

# 设置壁纸
1. yay -S feh
2. 进入~/.config/i3/config
3.
```
# 壁纸设定
exec_always --no-startup-id feh --bg-scale ~/url
```

#  安装谷歌云Linux版本
1. yay -S gdrive
2. https://github.com/gdrive-org/gdrive

# 设置i3容器边框样式
1. 进入~/.config/i3/config
2. 在末尾添加
```
#===============设置窗口边框===============
new_window none
bindsym $mod+t border normal
bindsym $mod+y border pixel 1
bindsym $mod+u border none
```

# 设置i3导航栏的图标字体
1. https://github.com/FortAwesome/Font-Awesome/releases
2. 解压进入把otf文件夹里的文件复制到/usr/share/fonts
3. fc-cache -fv
4. 进入~/.config/i3/config
5. 把想要图标的粘帖进去(https://fontawesome.com/cheatsheet?from=io)

# 设置平方字体
1. http://www.epinv.com/post/9478.html
2. 复制到/usr/share/fonts/TTF (这里要注意要在~/.xprofile中设置export LC_ALL="zh_CN.UTF-8")
3. 进入~/.config/i3/config
4. 修改font
```
font pango:PingFang Sc Light 8

```

# 安装程序主题,图标,鼠标央视
1. yay -S lxappearance
2. sudo vim .gtkrc-2.0
```
gtk-font-name=PingFang SC Regular 6
```
3.sudo vim .config/gtk-3.0/settings.ini
```
gtk-font-name=PingFang SC Regular 6
```
4. yay -S adapta-gtk-theme (主题)
5. yay -S papirus-icon-theme-git(图标)
6. yay -S bibata-cursor-theme(光标样式)
7. 然后在lxappearance选择就行了

# 安装polybar
1. yay -S polybar
2. install -Dm644 /usr/share/doc/polybar/config $HOME/.config/polybar/config  (这里需要自己找到自己的polybar/config)
3. polybar example (运行示例)


# 配置polybar
1. 写一个启动脚本,避免每次都polybar example
2. vim $HOME/.config/polybar/launch.sh
```shell
#!/usr/bin/zsh

# Terminate already running bar instances
killall -q polybar

# Wait until the processes have been shut down
while pgrep -u $UID -x polybar >/dev/null; do sleep 1; done

# Launch bar1 and bar2
polybar example -r &
```
3. chmod +x $HOME/.config/polybar/launch.sh
4. 进入~/.config/i3/config
```
# 自启动polybar
exec_always --no-startup-id $HOME/.config/polybar/launch.sh
```
5. 把i3默认bar禁止掉
```
#bar {
#        status_command i3status
#}
```
