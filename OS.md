## 进程间通信方式

信号

信号量

管道：redis中rdb、aof

消息队列

socket（不同主机）

共享内存

- 为什么需要通信？

> 操作系统是虚拟内存系统，每个进程看到的内存空间是独立的，独享用户空间，共享内核空间。所以借助内核空间，开辟缓冲区或者映射出来内核空间用来共享。

- 通信分类

1、同步 2、数据传递 3、消息共享

## 线程安全和不安全

不安全因素：全局变量、静态变量

解决方法：加锁（暴力方法）

## 死锁

竞争资源导致进程阻塞，都无法推进

### 产生条件

- 互斥：对必须互斥使用的资源进行争抢
- 不可剥夺：资源在未使用完时，不可被别的进程抢走
- 请求和保持：申请的资源被夺走，但是又不释放自己的资源
- 循环等待：存在循环等待链

## 进程、线程和协程

### 进程和线程的区别

根本区别：进程是操作系统资源分配的基本单位，而线程是处理器任务调度和执行的基本单位

资源开销：每个进程都有独立的代码和数据空间（程序上下文），程序之间的切换会有较大的开销；线程可以看做轻量级的进程，同一类线程共享代码和数据空间，每个线程都有自己独立的运行栈和程序计数器（PC），线程之间切换的开销小。

包含关系：如果一个进程内有多个线程，则执行过程不是一条线的，而是多条线（线程）共同完成的；线程是进程的一部分，所以线程也被称为轻权进程或者轻量级进程。

内存分配：同一进程的线程共享本进程的地址空间和资源，而进程之间的地址空间和资源是相互独立的

影响关系：一个进程崩溃后，在保护模式下不会对其他进程产生影响，但是一个线程崩溃整个进程都死掉。所以多进程要比多线程健壮。

执行过程：每个独立的进程有程序运行的入口、顺序执行序列和程序出口。但是线程不能独立执行，必须依存在应用程序中，由应用程序提供多个线程执行控制，两者均可并发执行

## GIT

Git合并分支的方式主要有两种，分别是Merge和Rebase。 解答思路：Merge是将两个分支的修改合并到一起，创建一个新的合并提交，保留了分支的commit历史。Rebase则是将当前分支的commit逐个应用到目标分支上，使得目标分支的commit历史更加线性清晰。选择Merge还是Rebase取决于项目的具体情况和团队的工作流程。 

## MySQL

关闭、开启服务的方法:

``` she
service start/stop mysql
```

## Nginx

启动方法：

首先进入/usr/local/nginx/sbin

`./nginx`

即可开启nginx

