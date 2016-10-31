---
title: Gradle系列四:联姻JAVA
date: 2016-10-31 20:55:49

tags:
- Gradle
- IT

---

本章为你简述下，Gradle是如何与Java联姻的
<!--more-->

## 联姻JVAA
### 快速牵手JAVA
- Gradle将把你从传统的构建流程中解放出来,针对不同的语言它有着各种不同的插件，通过插件封装了基本流程
 > ![插件与基本流程] (http://7xsh7v.com2.z0.glb.clouddn.com/6.png)

- 如何使用JAVA插件？只要一行代码
```
apply plugin: 'java'
```
这就定义了一个java项目的全部，自动添加很多task
Gradle 会在 src/main/java 目录下寻找产品代码，在 src/test/java 寻找测试代码 。 
另外在 src/main/resources 包含了资源的 JAR 文件, src/test/resources 包含了运行测试。
所有的输出都在 build 目录下，JAR 在 build/libs 目录下


### 外部依赖
 和Maven一样，我们也可以使用maven repository
```
apply plugin: 'java'

repositories {
	mavenCentral()
}

dependencies {
        compile group: 'org.springframework', name: 'spring-core', version: '4.2.5.RELEASE'
        compile group: 'org.hibernate', name: 'hibernate-core', version: '3.6.7.Final'
        testCompile group: 'junit', name: 'junit', version: '4.+'
    }

```
Maven respository网站上已经提供了各种工具包的依赖，稍微改变下格式描述就可以了
（下面将会介绍到Groovy插件，它更强大是java插件的扩展）
![获取依赖](http://7xsh7v.com2.z0.glb.clouddn.com/7.png)

**执行 gradle build **
![bulid结果](http://7xsh7v.com2.z0.glb.clouddn.com/8.png)


### 一个groovy项目
- 需要注意的是一个groovy项目也是一个java项目
```
apply plugin: 'groovy'

repositories {
	mavenCentral()
}

dependencies {
        compile 'org.codehaus.groovy:groovy-all:2.3.6'
        compile 'commons-logging:commons-logging:1.2'
        compile 'org.springframework:spring-core:4.2.5.RELEASE'
        testCompile 'junit:junit:4.11'
    }

```
比之前的java插件更强大哦, 而且依赖的描述也更简单了，从respository中直接copy就行了









