---
title: Gradle系列六：依赖管理与多项目构建
date: 2016-10-31 21:07:02
tags:
- Gradle
- IT

---

本章为你介绍Gradle的依赖管理原理，仓库配置，多项目
<!--more-->
### 依赖管理概述
**Gradle摒弃了Ivy和Maven依赖管理工具的一切缺点，更注重性能、构建可靠性以及可重复性**
- 通常开发都需要依赖一些流行的开源框架包，避免自己重复发明轮子
- 随着项目的增大，依赖的模块和第三方类库会越来越多，如何组织与管理就显得尤为重要
- 自动化依赖管理可以解决传递性依赖，版本管理等问题
- 搭建内部仓库解决中央仓库的单点依赖 Sonatype Nexus, JFrog, Artifactory
- Gradle提供了很有价值的依赖报告

**一个自动化依赖结构图**
![自动化依赖管理](http://7xsh7v.com1.z0.glb.clouddn.com/13.png)

### 依赖配置
- 通过project实例添加和访问配置
- 每个项目都有一个ConfigurationContainer类的容器来管理相应的配置
- 定义一个Cargio类配置的例子
```
configurations{
        cagro {
            description = 'classpath for cargo Ant tasks'
            visible = false
        }
    }
```

### 声明依赖
- 外部模块依赖
- 项目依赖
- 文件依赖
- 客户端模块依赖
- Gradle运行时依赖

**每个Gradle项目都有依赖处理器实例，由DependencyHandler接口来表示**

### 外部依赖
#### 依赖属性
- group 通常用来标示一个组织，公司或者项目
- name  唯一标识
- version 版本号 一般都包含主版本和次版本
- classifier  用来区分相同group，name，version工件，但使用环境上有所区别

例子：
![依赖属性例子](http://7xsh7v.com1.z0.glb.clouddn.com/10.png)


#### 依赖标记
有两种标识方式
- 使用map结构形式，显示标记出group，name，version
```
cargo group: cargoGroup,name:' cargo-core-uberjar', vewrsion: cargoVersion
```
- 使用字符串简写形式，类似上一节Hibernate的例子

**使用gradle dependencise 命令来查看详细依赖报告**

#### 排除传递依赖
由于Gradle会自动管理传递依赖，如果你需要可以主动排除一些自动的依赖

- 排除一个传递依赖
```
dependencies{
    cargo('org.codehaus.cargo:cargo-ant:1.3.1'){
        exclude(group:'xml-apis', module:'xml-apis')
    }
    cargo 'xml-apis:xml-apis:2.0.2'
}
```
- 排除所有传递依赖
```
dependencies{
    cargo('org.codehaus.cargo:cargo-ant:1.3.1'){
        transitive = false
    }
}
```
**Gradle还支持最新版本依赖，又叫动态版本依赖，当然正式环境下还是不要用这个功能了**

###文件依赖
 适用于从现有Ant或者Maven管理方式转变到Gradle时，或者本地稳定的开发包依赖
```
dependencies{
    cargo fileTree(dir: "lib路径" ，include: ' *.jar ')
}
```

### 使用和配置仓库
- 定义仓库的接口类 RepositoryHandler
- 支持Maven仓库，Ivy仓库，扁平的目录仓库

### Maven仓库
- 中央仓库 mavenCentral()
- 本地仓库 mavenLocal()  慎用
- 自定义仓库 maven() 和mavenRepo()

```
dependencies{
    mavenCentral()
    maven{
        name  'Custom Maven Repository'
        url 'http://ziniuxiaozhu.com/nexus/public'
    }
}
```

### Ivy仓库
- 相对Maven来说可以自定义布局
- 仓库依赖元数据存储在ivy.xml中

**具体语法可参考官方文档**

### 扁平目录仓库
- 只有JAR文件，没有元数据
- 适合经常手动维护项目中的类库，或者项目迁移的时候
- 声明依赖智能使用name和version这两个属性

一个从falt目录仓库取Cargo依赖声明例子:
```

repositories{
    flatDir(dir: " ${System.properties['user.home']}/libs/cargo", name: 'Local libs directory')
}

dependencies{
    cargo name : 'activetion', version:'1.1'
    catgo name: 'ant', version:'1.7.1'
    cargo  ':xml-apis:1.3.1', ' : jaxen:1.0-FCS'
}
```

### 本地依赖缓存
- Gradle会缓存从仓库下载的二进制文件，该目录根据不同的版本，路径可能不同
- 存储依赖来源，当来源发生变化，能够保证构建的可靠
- 减少到远程仓库的传输
- 比较本地仓库和远程仓库，减少工件的下载
- 支持离线模式，在你无法联网时采用本地缓存来构建


### 常见依赖问题
**如果你项目有很多依赖，而且你选择自动解决传递性依赖，那么版本冲突几乎是不可避免的**
- 应对版本冲突
设置当遇到版本冲突时，构建失败
```
 configuration.cargo.resolutionStrategy{
    failOnVersionConfilct()
}
```
- 强制制定一个版本
```
 configuration.cargo.resolutionStrategy{
   force ' org.codehaus.cargo:cargo:cargo-ant:1.3.0'
}
```
- 使用依赖观察报告
- 刷新缓存
 > 可以手动刷新，可以不缓存快照版本的依赖包


### 模块化项目构建
#### 规划号自己项目的模块
- 根据高内聚低耦合的设计思想，设计符合自己项目的模块
- 一般会有web层，控制层，服务层，数据层
- 每个层下面的路径可以自定义，只后在构建文件build.gradle中说明就行
  > 建议采用默认的就行

大概的结构图如下：
![项目结构示意图](http://7xsh7v.com1.z0.glb.clouddn.com/11.png)
每个模块下都有二个文件： build.gradle 和 setting.gradle


### 理解setting文件
- 对项目根目录下的setting.gradle文件进行配置多模块项目
```
rootProject.name = 'ziniuxiaozhu'
include 'common'
include 'service'
include 'webapp'
```
- setting文件的执行是在初始化阶段
回顾下gradle的三个生命周期
![构建生命周期](http://7xsh7v.com1.z0.glb.clouddn.com/12.png)

- 初始化的时候先从本模块下的setting文件中寻找，再到根目录下去寻找
 > 也可以通过命令行参数控制setting搜索行为
- 支持分层布局和扁平布局，当然个人推荐分层的，能够更细粒度控制组件的建模























 












