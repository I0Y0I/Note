安装的是
```bash
yay -S wechat-bin
```
发现输入法不能正常使用，在`~/.profile`中添加
```profile
QT_IM_MODULE=fcitx
GTK_IM_MODULE=fcitx
```
后可以正常使用，但KDE on Wayland不推荐这样添加，因此在KDE启动菜单`wechat.desktop`设置中单独添加这两条环境变量，解决。