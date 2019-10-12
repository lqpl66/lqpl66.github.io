---
title: poi版本小结
date: 2019-10-12 14:35
author: lqpl66
categories: 
   - 常遇问题梳理
tags:  
   - bug整理
---

####  起因

​	工作中遇到excel报表的导出和导入问题，原先项目采用原生的poi处理，导入导出功能暂无异常；

后来业务需求导出的excel的表头为多级和动态，所以采用easypoi或easyexcel这两种第三方工具包；

但因依赖的核心包poi相关版本由原先的3.15beta2版本升级至4.0版本，导致很多问题，先梳理一下以防再次入坑。

#### 版本依赖

#####  poi-3.15相关依赖

```java
poi-3.15.jar
poi-ooxml-3.15.jar
poi-schemas-3.15.jar
```

##### 3.17

```java 
poi-3.17.jar
poi-ooxml-3.17.jar
poi-schemas-3.17.jar
```

##### 4.0

```java
 使用POI 4.0实现，导出格式为.xlsx

 poi-4.0.0.jar
 poi-ooxml-4.0.0.jar
 poi-ooxml-schemas-4.0.0.jar

以及下载的POI自身实现引用的jar包

 commons-codec-1.11.jar
 commons-collections4-4.2.jar

额外还需要

 xmlbeans-3.0.2.jar
 commons-compress-1.18.jar
```

#### easypoi 版本

3.0版本依赖poi核心jar版本3.*即可

4.0版本则需要poi4.0版本

#### poi版本遇到差异性

现遇到的是单元格cell样式和类型的差异性，所以升级需慎重。

