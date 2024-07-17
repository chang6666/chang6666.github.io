---
title: mac没有ll命令解决方案
date: 2021-01-05 08:29:53
categories:
- IDEA
tags:
- IDEA
---

idea启动项目报错:Command line is too long. Shorten command line for className or also defaultconfiguration


在该项目文件夹.idea/workspace.xml中找到


```xml
<component name="PropertiesComponent">

  ...

</component>
```

然后在其中添加:


```xml
<property name="dynamic.classpath" value="true" />
```

问题得到解决。