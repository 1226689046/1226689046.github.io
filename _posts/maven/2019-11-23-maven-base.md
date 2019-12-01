---
layout: post
title: maven的基本配置
tags:
- maven
categories: maven
description: maven的基本配置，以后不许再去搜索引擎搜索了,记录java maven 项目的目录结构，以及基本的操作
---

# maven的基本配置

以前对于Spring的学习，一直都是在不断的重复基础的内容，没有进行记录，这是一个非常不好的习惯，如果进行记录了，下一次就可以直接到这里看，而不用重复的进行无用的搜索。总结真的是一个很好的习惯

# java maven de的目录结构

> src
>
> > main
> >
> > > java
> > >
> > > resource
> > >
> > > webapp

## 遇到的坑

1. 如果pom文件导入了，但是还是提醒包不存在，则reimport一下就行了