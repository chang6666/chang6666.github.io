---
title: 如何忽略/隐藏/关闭macOS Catalina/Mojave的更新通知
date: 2021-01-03 08:29:53
categories:
- Mac
tags:
- Mac
---

macOS Catalina 10.15最大的差异就是不支持32位软件，如果是工作使用请确认自己的生产软件是否都兼容再升级。

隐藏/关闭更新通知依次打开 启动台-其他-终端，输入以下神秘代码，Enter后输入用户密码即可。

```
#隐藏/关闭Catalina更新
sudo softwareupdate --ignore "macOS Catalina"

#隐藏/关闭Mojave更新
sudo softwareupdate --ignore "macOS Mojave"
```

隐藏/关闭小版本更新

```
#10.15.4
sudo softwareupdate --ignore "macOS Catalina 10.15.4 Update"

#10.14.5
sudo softwareupdate --ignore "macOS Mojave 10.14.5 Update"
```

关闭偏好设置内的系统更新提示小红点

```
defaults write com.apple.systempreferences AttentionPrefBundleIDs 0 && killall Dock
```

将来要是又想在线更新了，再次输入以下神秘代码即可。

```
sudo softwareupdate --reset-ignored
```