---
title: Modal
date: 2016-06-06 22:06:29
tags: iOS学习
---

### 简介

* 在iPhone开发中
* Modal是一种常见的切换控制器的方式
* 默认是从屏幕底部往上弹出，直到完全盖住后面的内容为止
* 在iPad开发中
* Modal的使用频率也是非常高的
* 对比iPhone开发，Modal在iPad开发中多了一些用法

### 呈现样式

* 什么叫呈现样式
* Modal出来的控制器，最终显示出来的样子
* Modal常见有4种呈现样式
* UIModalPresentationFullScreen ：全屏显示（默认）
* UIModalPresentationPageSheet
* 宽度：竖屏时的宽度（768）
* 高度：当前屏幕的高度（填充整个高度）
* UIModalPresentationFormSheet ：占据屏幕中间的一小块
* UIModalPresentationCurrentContext ：跟随父控制器的呈现样式

### 过渡样式

* 什么叫过渡样式
* Modal出来的控制器，是以怎样的动画呈现出来
* Modal一共4种过渡样式
* UIModalTransitionStyleCoverVertical ：从底部往上钻（默认）
* UIModalTransitionStyleFlipHorizontal ：三维翻转
* UIModalTransitionStyleCrossDissolve ：淡入淡出
* UIModalTransitionStylePartialCurl ：翻页（只显示部分，使用前提：呈现样式必须是UIModalPresentationFullScreen）

