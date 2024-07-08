---
layout:     post
title:      golang管道
subtitle:   channel基础与底层原理
date:       2024-07-08
author:     HDW
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - golang
---

#### 基础部分

GO中通道相当于FIFO的队列，创建方式如下：
```
ch1 := make(chan int)      // 无缓冲通道
ch2 := make(chan int, 3)   // 有缓冲通道
ch3 := make(chan<- int, 1) // 单向通道：只能发送不能接收
ch4 := make(<-chan int, 1) // 单向通道：只能接收不能发送
```

使用方式（一般在不同协程中，用于消息传输）：
```
ch := make(chan string)
ch <- "hhh123"     // 发送进通道
go func(){
    s := <-ch        // 从通道中获取
}()
close(ch)
```
发送与接受的内部操作：发送操作包括“复制元素值”和“放入通道”2步，接收操作包括“复制通道内的元素值”、“放置副本到接收方”和“删掉原值”3步。
使用完channel后，记得close掉，否则可能会引起内存泄漏。

#### 底层原理

<https://zhuanlan.zhihu.com/p/587718836>

