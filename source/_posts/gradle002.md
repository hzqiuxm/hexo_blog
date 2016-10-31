---
title: Gradle系列二：入门与安装
date: 2016-10-31 10:10:31
tags:
- Gradle
- IT

---
通过这一章节，你讲学会如何安装Gradle以及简单的任务构建与操作
<!--more-->

## 入门与安装
### 下载安装
- 官方下载地址: https://gradle.org/gradle-download/
- 安装就是配置环境变量，增加GRADLE_HOME环境变量，值为gradle-2.12\bin所在目录
    > windows下直接在环境变量对话框中定义，类linux系统
        export GRADE_HOME = xxxx/xxxx    
        export PATH=$PATH:$GRADLE_HOME/bin
- 验证是否安装成功
 ![check install](http://7xsh7v.com2.z0.glb.clouddn.com/1.png)
- 来一个gradle的hello world
**新建一个文件 build.gradle**
```
task helloWorld{

	task startSession << {
		println 'Welcome to gradle world!'
	}

	3.times {
		task "genGradle$it" << {
			println 'genGradle rocks'
		}
	}
		
	genGradle0.dependsOn startSession
	genGradle2.dependsOn genGradle1, genGradle0
	
	task groupGradles(dependsOn: genGradle2)

}

```

- 执行：gradle -q groupGradles
![run helloworld](http://7xsh7v.com2.z0.glb.clouddn.com/2.png)


### 程序简单说明
- 这里的task和Ant的targets是对等的
- << 符号是doLast的alias，doFirst 和 doLast 可以多次执行调用。他们在开始或结束的 task 动作清单中添加动作。task 执行时，按动作列表的顺序执行的动作。操作符 << 仅仅是 doLast 的别名。
- -q是代表quiet模式，不生成构建编译过程的日志信息

### 其他技巧
- 列出项目中所有的task
```
gradle -q tasks --all
```
- 在执行时排除一个任务
```
gradle groupGradles -x genGradle0
```
- 任务名字采用缩写
```
gradle groupHello 等价于  gradle gH
gradle groupGradle0 等价于 gradle gG0
```
- 常用的命令行选项
```
-b 指定构建脚本名字，某人build.gradle
-i 执行时候输出详细日志信息
-q 减少构建出错时打印的日志信息
--help 打印出所有可用的命令行选项

```
- 以后台守护进行方式运行gradle
```
gradle groupGradles --daemon
```






