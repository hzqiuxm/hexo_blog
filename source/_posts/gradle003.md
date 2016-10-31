---
title: gradle系列三：脚本基础
date: 2016-10-31 10:15:52
tags:
- Gradle
- IT

---
欢迎来到Gradle系列教材，这一章节讲的是脚本基础相关知识：Groovy ......
<!--more-->
## 脚本基础
### 看看灵活的Groovy例子
- 编写脚本build.gradle
```
task upper << {
	println '---- Begin in upper ----'
	String strSome = 'my_nAme'
	println 'Original: ' + strSome
	println 'Upper: ' + strSome.toUpperCase()
	println '---- End in upper ----'
}	

task hello(dependsOn: upper) << {
	println " I'm dependsOn upper"
}
```
执行下 gradle -q hello
![执行结果](http://7xsh7v.com2.z0.glb.clouddn.com/3.png)


- 更多的脚本技巧请参考：
 [极客学员Grade2用户指南](http://wiki.jikexueyuan.com/project/gradle-2-user-guide/build-script-basics.html)

### 默认task设置
```
defaultTasks 'clean', 'run'

task upper << {
	println '---- Begin in upper ----'
	String strSome = 'my_nAme'
	println 'Original: ' + strSome
	println 'Upper: ' + strSome.toUpperCase()
	println '---- End in upper ----'
}	

task hello(dependsOn: upper) << {
	println " I'm dependsOn upper"
}

task clean << {
	println 'This is a default task clean '
}

task run << {
	println 'This is a default task run '
}

```
**执行 gradle -q 结果**
![执行结果](http://7xsh7v.com2.z0.glb.clouddn.com/4.png)

### DAG配置
```
task destribution <<{
	println "We build the zip with version = $version "
}

task release(dependsOn:'destribution') << {
	println 'We release now ......'
}

gradle.taskGraph.whenReady{
	taskGraph -> 
	if (taskGraph.hasTask(release)) {
		version = '1.0'
	}else{
		version = '1.0-SNAPSHOT'
	}
}

```
**注意执行不同的TASK，输出结果是不一样的**
 ![执行结果](http://7xsh7v.com2.z0.glb.clouddn.com/5.png)













