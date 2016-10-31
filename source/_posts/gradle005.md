---
title: Gradle系列五：开始一个WEB应用
date: 2016-10-31 21:01:59
tags:
- Gradle
- IT

---

本章为你简述下，通过Gradle快速开始一个WEB应用
<!--more-->
## 开始一个WEB应用
### 构建WAR文件
- 使用war插件来构建war文件
```
apply plugin: 'war'
```
当运行 *gradle build*时就会编译、测试、打包工程一个war文件

- 运行应用插件
```
apply plugin: 'jetty'
```
当你运行 *gradle jettyRun* 的时候，应用将会运行在一个内嵌的jetty web容器中

### gradle的Task执行规则
- 同个构建可以执行多个task，但每个task只能被执行一次，不管它是在哪个阶段被执行
```
    task compile << {
        println 'compiling source'
    }

    task compileTest(dependsOn: compile) << {
        println 'compiling unit tests'
    }

    task test(dependsOn: [compile, compileTest]) << {
        println 'running unit tests'
    }

    task dist(dependsOn: [compile, test]) << {
        println 'building the distribution'
    }
```
执行 *gradle dist*结果如下
![任务执行规则](http://7xsh7v.com2.z0.glb.clouddn.com/9.png)

### 构建块
注意每个Gradle包含了3个基本构建块：project、task、property






