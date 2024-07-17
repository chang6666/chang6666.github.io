---
title: MacOS下禁用新版 Microsoft Edge 自动更新
date: 2022-08-07 08:30:53
categories:

- Mac
- Edge

tags:

- Mac
- Edge

---

# MacOS下禁用新版 Microsoft Edge 自动更新



找到Edge的更新程序：

```
➜ ls /Library/Application\ Support/Microsoft/MAU2.0/
Microsoft AutoUpdate.app
```
查看一下该文件的权限：

```
➜ ls -al /Library/Application\ Support/Microsoft/MAU2.0/
total 0
drwxr-xr-x  3 root  wheel  96  4 15 20:05 .
drwxr-xr-x  3 root  wheel  96  4 15 20:05 ..
drwxrwxr-x  3 root  wheel  96  4 15 20:05 Microsoft AutoUpdate.app
```
可以看到当前权限是 `775`。

修改其权限为 `644`，使其不能执行：

```
➜ sudo chmod -R 644 /Library/Application\ Support/Microsoft/MAU2.0/Microsoft\ AutoUpdate.app
Password:
```
后期如果想要改回来以保持Edge的最新版本的话，只需要执行：


```
sudo chmod -R 775 /Library/Application\ Support/Microsoft/MAU2.0/Microsoft\ AutoUpdate.app 
```

让其恢复可执行权限即可。