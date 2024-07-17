---
title: VScode配置Java以及maven环境
date: 2021-06-01 08:29:53
categories:
- Java
- VScode
tags:
- Java
- VScode
---

#### 1. 进入vscode的settings.json

-   文件 > 首选项 > 设置 > 在 setting.json中编辑
![image-20211029151307091.png](https://raw.githubusercontent.com/ChangNingbo/blog_img/main/img202208081421015.png)
#### 2. 配置jdk
在 setting.json中添加以下内容
```
 "java.home": "/Library/Java/JavaVirtualMachines/zulu-8.jdk/Contents/Home",
 "concourse.ls.java.home": "/Library/Java/JavaVirtualMachines/zulu-8.jdk/Contents/Home",
```
![image-20211029152723784-16354924461152.png](https://raw.githubusercontent.com/ChangNingbo/blog_img/main/img202208081421331.png)
##### 2.1 配置多个jdk版本
同时添加下列内容
```
 "java.configuration.runtimes": [
    {
      "name": "JavaSE-1.8",
      "path": "/Library/Java/JavaVirtualMachines/zulu-8.jdk/Contents/Home",
      "sources": "/Library/Java/JavaVirtualMachines/zulu-8.jdk/Contents/Home/src.zip",
      "default": true
    },
    {
      "name": "JavaSE-11",
      "path": "/Library/Java/JavaVirtualMachines/zulu-11.jdk/Contents/Home",
      "sources": "/Library/Java/JavaVirtualMachines/zulu-11.jdk/Contents/Home/lib/src.zip"
    },
    {
      "name": "JavaSE-17",
      "path": "/Library/Java/JavaVirtualMachines/zulu-17.jdk/Contents/Home",
      "sources": "/Library/Java/JavaVirtualMachines/zulu-17.jdk/Contents/Home/lib/src.zip"
    }
  ],
```
运行时名称必须是以下之一：“ J2SE-1.5”，“ JavaSE-1.6”，“ JavaSE-1.7”，“ JavaSE-1.8”，“ JavaSE-9”，“ JavaSE-10”，“ JavaSE-11”，“ JavaSE-12”，“ JavaSE-13”，“ JavaSE-14”。 我们将使用每个受支持的JDK版本来更新列表。

#### 3. 配置maven路径和settings.xml

```
// 配置maven路径
"maven.executable.path": "D:\software\worksoftware\apache-maven-3.6.3\bin\mvn.cmd",

// 配置maven的settings.xml
"maven.settingsFile": "D:/JetBrains/apache-maven-3.8.1/conf/settings.xml",
"java.configuration.maven.userSettings": "D:/JetBrains/apache-maven-3.8.1/conf/settings.xml",
"maven.terminal.customEnv": [
    {
      "environmentVariable": "JAVA_HOME",
      "value": "/Library/Java/JavaVirtualMachines/zulu-8.jdk/Contents/Home"
    }
  ],
```

至此 Vscode配置java、maven完成。