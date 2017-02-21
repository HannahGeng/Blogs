---
title: 关于GCD
date: 2017-01-10 22:44:54
tags: iOS学习
---

#### 1.GCD基本知识

01.两个核心概念——队列和任务

> 任务：执行什么操作
队列：用来存放任务

02.同步函数和异步函数

#### 2.GCD基本使用

GCD的使用就两个步骤：**定制任务确定想做的事、将任务添加到队列中**：

> 1.GCD会自动将队列中的任务取出，放到对应的线程中执行
2.任务的取出遵循队列的FIFO原则：先进先出，后进后出 

    01.异步函数+并发队列：开启多条线程，并发执行任务
    
    02.异步函数+串行对列：开启一条线程，串行执行任务
    
    03.同步函数+并发队列：不开线程，并发执行任务
    
    04.同步函数+串行对列：不开线程，串行执行任务
    
    05.异步函数+主队列：不开线程，在主队列中串行执行任务
    
    06.同步函数+主队列：不开线程，串行执行任务（注意死锁发生）
    
    07.注意同步函数和异步函数在执行顺序上的差异

#### 3.GCD线程间通信

```
//0.获取一个全局的队列
    dispatch_queue_t queue = dispatch_get_global_queue(0, 0);

    //1.先开启一个线程，把下载图片的操作放在子线程中处理
    dispatch_async(queue, ^{

       //2.下载图片
        NSURL *url = [NSURL URLWithString:@"http://h.hiphotos.baidu.com/zhidao/pic/item/6a63f6246b600c3320b14bb3184c510fd8f9a185.jpg"];
        NSData *data = [NSData dataWithContentsOfURL:url];
        UIImage *image = [UIImage imageWithData:data];

        NSLog(@"下载操作所在的线程--%@",[NSThread currentThread]);

        //3.回到主线程刷新UI
        dispatch_async(dispatch_get_main_queue(), ^{
           self.imageView.image = image;
           //打印查看当前线程
            NSLog(@"刷新UI---%@",[NSThread currentThread]);
        });

    });
```
#### 4.GCD其他常用函数

```
01 栅栏函数（控制任务的执行顺序）
    dispatch_barrier_async(queue, ^{
        NSLog(@"--dispatch_barrier_async-");
    });

    02 延迟执行（延迟·控制在哪个线程执行）
      dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(2.0 * NSEC_PER_SEC)), dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
        NSLog(@"---%@",[NSThread currentThread]);
    });

    03 一次性代码（注意不能放到懒加载）
    -(void)once
    {
        //整个程序运行过程中只会执行一次
        //onceToken用来记录该部分的代码是否被执行过
        static dispatch_once_t onceToken;
        dispatch_once(&onceToken, ^{

            NSLog(@"-----");
        });
    }

    04 快速迭代（开多个线程并发完成迭代操作）
       dispatch_apply(subpaths.count, queue, ^(size_t index) {
    });

    05 队列组（同栅栏函数）
    //创建队列组
    dispatch_group_t group = dispatch_group_create();
    //队列组中的任务执行完毕之后，执行该函数
    dispatch_group_notify(dispatch_group_t group,dispatch_queue_t queue,dispatch_block_t block);

    06进入群组和离开群组
    dispatch_group_enter(group);//执行该函数后，后面异步执行的block会被gruop监听
    dispatch_group_leave(group);//异步block中，所有的任务都执行完毕，最后离开群组
    //注意：dispatch_group_enter|dispatch_group_leave必须成对使用
```

#### 5.容易混淆的术语

有四个术语容易混淆：**同步、异步、并发、串行**

* 同步和异步主要影响：能不能开启新的线程

    *   同步：只是在当前线程中执行任务，不具备开启新线程的能力
    *   异步：可以在新的线程中执行任务，具备开启新线程的能力  
     
* 并发和串行主要影响：任务的执行方式

    * 并发：允许多个任务并发（同时）执行
    * 串行：一个任务执行完毕后，在执行下一个任务
             


