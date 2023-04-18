Linux高级1

# ·4种常见的IO模型：

同步阻塞 同步非堵塞 IO多路复用 异步IO

https://www.cnblogs.com/straybirds/p/9479158.html

操作系统四个基本特性：并发 共享 虚拟 异步

# 1.内存

(1).连续内存分配策略：首次适配 最佳适配 最差适配

https://blog.csdn.net/iwanderu/article/details/103934647

(2).非连续内存分配策略：分段(分为堆、栈等区域)和分页(分为大小固定的页)

(3).虚拟内存：

原理：程序的局部性原理。

当用户程序进入内存运行的时候，不是将所有的页面都装入内存，而是装入部分就可以启动程序运行。在运行过程中没如果发现要运行的程序或访问的数据不再内存里，就想系统发送缺页中断请求，系统将外存中的页面调入内存，使得程序继续运行。

(4).虚拟内存中的页面置换算法：

最优页面置换算法        先进先出算法        最近最久未使用算法         最少使用算法        时钟页面置换算法

# 2.进程

## 1.特点：动态性 并发性 独立性 并发性

## 2.进程和线程的区别：

线程（英语：thread）是操作系统能够进行运算调度的最小单位。

进程（Process）是系统进行资源分配和调度的基本单位，是操作系统结构的基础。

## 3.进程 的生命周期：

进程创建-进程运行-进程等待-进程唤醒-进程结束

## 4.进程的状态变化：

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MWQyYWRhMjYwYzkxYzY5N2M3NmYxM2NhYTIwYzkwMGZfYnpYQVFZVHV0RUNvSzBpOFJOYWZOQU5ETkVCc2VMOWVfVG9rZW46Ym94Y243U3ZSWlpDSExTSnFsUm45d3oyNnllXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

## (5).进程调度算法

https://www.cnblogs.com/szitcast/p/10927375.html

FCFS(先来先服务)

SPN(短进程优先)

HRRN(最高响应比优先)

轮询

公平调度

# 3.线程(线程控制块TCB)

## (1)优点：

一个进程可以有多个线程

各个线程可以并发执行

各个线程之间可以共享资源

## (2).缺点：一个线程崩溃，会导致其所属进程的所有线程崩溃

## (3).进程和线程的比较

(1).进程是资源分配的基本单位，线程是CPU调度的基本单位

(2).进程拥有完整的资源平台，而线程仅独享必不可少的资源如寄存器和栈

(3).线程同样具有就绪、堵塞、执行三种状态以及状态之间的转换

(4).线程能减少并发执行的时间和空间开销

线程创建使劲比进程短                                线程终止时间比进程短

线程切换比进程切换时间短                        同一进程可以不通过内核就可以通信

# 4.快捷键

## Linux命令

无法复制加载中的内容

## Linux快捷键

无法复制加载中的内容

## Vim快捷键

无法复制加载中的内容

# 5. linux高级

## 1.GDB调试器

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=YTlmZTBkMDk2MDczZDlhNTZiMzk4M2VlYWY3ZTA0MDRfbFBBeG40SWRieklyYlhPbU9tUUdOSlE1UlVvSkZKSGVfVG9rZW46Ym94Y25yMDNrMktvSThzajFONEdVZ096ZDVlXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MDk4OTRhZDgzNmJkMDRjMWU0YTlkMTRiMzIwYzA0MGRfdFpyOVhVRVBXWVl5ZTgzNklEYnRjQThWNmRpZHM4RllfVG9rZW46Ym94Y25VOU5NMFcwbVg2U0lBdVZFNzdNOUVnXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=NTczMzAwZTQ4ODRlNGRlYTliYTlhZWI0Njk0ZTAyNTJfdmpkY1JMVkNxT011c3Q5VUJvcm83bU16MHNORzdWeW1fVG9rZW46Ym94Y25iMWxhREpDWEF1Q2hnazBTbThPa1FjXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=NDI0YzkyNGIyMTQzODMwNmQ1ZWQzOGQxNWNmYTI4NGRfZ3RvbWtxTmo2eGRqbXdjVlpQcW1HbzN2QW5nQjN3bjJfVG9rZW46Ym94Y25YaEpZWmdteDMxZFJHbUFGQzRhQzFmXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

## 2.Makefile

### 1.Makefile三要素：

目标：依赖

执行命令

```C%2B%2B
                app:main.c
    gcc main.c -o app
```

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MGI4MWFiZmQyM2Y1YTViY2U0ZmE0MjU0MjM1ZjE3MzVfTTVJOUJlVzZpVTJobXc1Slk5SURTOFc4eGJCT1ZsdGJfVG9rZW46Ym94Y25Bb3k1UURGS0t5TnpPOThySGN6b0RkXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDk0MDBjMjY5ZmQ4ZTM2NjJmMjQ3OWI1Y2Q5ZmE5OTZfMEROMHpVdndhZjBrQUhqbURVMlg2cUg0RXdRbjBHaHFfVG9rZW46Ym94Y25jS3Qycjc4RnU4Z08wM096bmthZVJjXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

```C%2B%2B
 -Wall //严格编译
-c //生产*.o文件
-I //头文件路径
-E //生成预处理信息
```

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTk3ZWMyZjMxZmJiYWNjM2MxNGFlOGEyZWNmZmU4YmFfcnRMTDE4OTA5M0kwa05EVVBuaHpMVXRmZ0J3NHdhdmhfVG9rZW46Ym94Y25TakdCbnYxQUZtUTd0SWpZY21lVXVjXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=ZmZlODdhMzc1YTcxZGI2MTA0MjA5ZWE4NTdiNmZkMGJfaFduSjlCTFpUOXh2YUIxdjU5ZDdzNnhaVWdxeHFvcDBfVG9rZW46Ym94Y25XckdzU1Rmc1Zic3F1V014ZGNPa0dlXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

## 3.文件IO

### 3.1 文件的操作

文件的操作都是针对文件描述符的，文件描述符是一个非负的整数。凡是带at的函数如果文件描述符fd=AT_FDCWD，则`pathname`是相对于当前工作目录来计算的。

#### 1.read/write/open：

https://blog.csdn.net/q1007729991/article/details/52563279

#### 2.文件描述符与lseek

https://blog.csdn.net/q1007729991/article/details/52564810

#### 3.fcntl

https://blog.csdn.net/q1007729991/article/details/52663630

## 4.文件和目录

### 1.四个stat函数：返回文件或者目录的信息结构：

https://blog.csdn.net/q1007729991/article/details/53366751

### 2.文件类型

\- 普通文件：最常见的文件类型，包含了某种形式的数据。至于这种数据是二进制还是文本，对内核无区别。普通文件的内容解释由具体的应用程序进行。

\- 目录文件：这种文件包含了其他文件的名字，以及指向这些文件有关信息的指针。 只有内核可以直接写目录文件（通常用户写目录文件都要通过内核）。对某个目录文件具有读权限的任何一个进程都可以读取该目录的内容

 \- 块特殊文件：这种类型的文件提供对设备（如磁盘）带缓冲的访问。每次访问以固定长度为单位进行。

 \- 字符特殊文件：这种类型的文件提供对设备不带缓冲的访问，每次访问长度可变。系统的所有设备，要么是字符特殊文件，要么是块特殊文件

 \- FIFO：这种类型的文件用于进程间通信，有时也称为命名管道

\- 套接字：这种类型的文件用于进程间的网络通信（也可用于单机上进程的非网络通信）

 \- 符号链接：这种类型的文件指向另一个文件

### 3.chmod() fchmod() fchmodat()

https://blog.csdn.net/q1007729991/article/details/53418504

### 4.chown() fchown() fchownat() lchown(）

https://blog.csdn.net/q1007729991/article/details/53423563

### 5.文件截断

```C%2B%2B
#include<unistd.h>
int truncate(const char*pathname,off_t length);
int ftruncate(int fd,off_t length);
```

 将一个文件的长度截断为length，如果文件超过length后面的则不能访问，没有超过，则补0

### 6.文件链接和取消链接

软连接和硬链接：

https://www.cnblogs.com/songgj/p/9115954.html

创建和取消硬链接：成功返回0 失败返回-1 link是创建一个硬链接 unlink可以删除pathname这个链接文件可以是软连接也可以是硬链接

### 7.创建删除和读目录

```C%2B%2B
#include<sys/stat.h>
int mkdir(const char*pathname,mode_t mode);
int mkdirat(int fd,const char *pathname,mode_t mode);
int rmdir(const char *pathname);
```

### 8.改变目录和获取目录

```C%2B%2B
 #include<unistd.h>
int chdir(const char *pathname);//改变目录 接目录名 成功0 失败-1
int fchdir(int fd);//到文件描述符所在的目录 成功0 失败-1
char *getcwd(char *buf,size_t size);//返回当前工作目录的名字 成功返回指向缓冲区的指针
```

## 5.标准IO库

### 1.打开一个流和读写流

```C%2B%2B
 // 打开流 成功返回文件指针 失败返回NULL
#include<stdio.h>
FILE *fopen(const char*restrict pathname,const char*restrict type);
FILE *freopen(const char*restrict pathname,const char*restrict type,FILE *restrict fp);
FILE *fdopen(int fd,const char*type);
// getc/fgetc/getchar函数：一次读一个字符：
int getc(FILE*fp);
int fgetc(FILE*fp);
int getchar(void);
// ferror/feof函数：查看是读文件出错，还是到达读文件遇到尾端 真 返回非0 假返回0
int ferror(FILE *fp);
int feof(FILE *fp);
// 一次写一个字符
int putc(int c,FILE*fp);
int fputc(int c,FILE*fp);
int putchar(int c);
//一次读一行字符：
char *fgets(char *restrict buf,int n, FILE* restrict fp);
char *gets(char *buf);
//一次写一行字符：
int fputs(const char* restrict str,FILE*restrict fp);
int puts(const char*str);
```

## 6.系统文件和数据

### 6.1进程基础

[https://mp.weixin.qq.com/s?__biz=MzIwNTc4NTEwOQ==&mid=2247487913&idx=1&sn=8c3f042c5a73ce9e49b21bc6cce2442e&chksm=972ac0d3a05d49c5904f8f10156de521b62c526c805b2b6f0c28814b4a09ff5a7c39d5a826dd&scene=126&sessionid=1583499218&key=d6997d945f68076eaba879d27c2a6415bcd058decd459ee78447c8309f340e276f6049a0e7b83a810056a232102ff48566e30d158985f9feb9bfdee793c4eb516a21a42679fb2cea43b1c320ea4f2873&ascene=1&uin=Mjg5OTQ2ODYxOA%3D%3D&devicetype=Windows+10&version=62080079&lang=zh_CN&exportkey=A2oisNBFXqsMXTLUk1up0Vc%3D&pass_ticket=5IAkAKLl3DVdBgeWOeME%2BUSyYdnfQifu%2BAyhsjVC637mhDw1%2BTcRXd%2F2SHmkAEBu](https://mp.weixin.qq.com/s?__biz=MzIwNTc4NTEwOQ==&mid=2247487913&idx=1&sn=8c3f042c5a73ce9e49b21bc6cce2442e&chksm=972ac0d3a05d49c5904f8f10156de521b62c526c805b2b6f0c28814b4a09ff5a7c39d5a826dd&scene=126&sessionid=1583499218&key=d6997d945f68076eaba879d27c2a6415bcd058decd459ee78447c8309f340e276f6049a0e7b83a810056a232102ff48566e30d158985f9feb9bfdee793c4eb516a21a42679fb2cea43b1c320ea4f2873&ascene=1&uin=Mjg5OTQ2ODYxOA==&devicetype=Windows+10&version=62080079&lang=zh_CN&exportkey=A2oisNBFXqsMXTLUk1up0Vc=&pass_ticket=5IAkAKLl3DVdBgeWOeME+USyYdnfQifu+AyhsjVC637mhDw1+TcRXd/2SHmkAEBu)

### 6.2进程的生存环境

1.什么是进程？

https://blog.csdn.net/alidada_blog/article/details/81811600

进程的功能开发者可以自定义，内核负责调度给该进程分配资源，所以说进程是一个系统调度的基本单位。内核在调度进程的时候需要分配内存空间和时间片(CPU使用权)这些资源。

2.什么是并发？

多进程工作，切换中断频率高，执行任务处理密集，能加快任务完成速度，多进程可以提高获取时间片的概率，进而缩短整个任务的时间周期。

3.进程的生存环境：

https://blog.csdn.net/Wjy2016/article/details/51878440

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MzdmYjhkNGUyMzM3NjZiYjIyZGNiZjY0NjYxYzg2MjhfMFBVZFhSbnV3TzJBWjJKY25zcUhMMGhRbmU3ZHl5UDlfVG9rZW46Ym94Y25IU3h4NDBKYXN5OWtxVXdnWDNBbDFkXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MGQ3MjdmMDAyOGZhYWRiNzBlMjNlNGJiNzI4NjQ4MThfdEdpQ0VYN3NrUjJFOElkdUJveThIeEJUeHJFeGdIUlJfVG9rZW46Ym94Y251bXNqQmpZQkNGQ3hSbGllWm5TV3BjXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

#### 6.2.1进程状态

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=Zjk5YjZiYjU5MDNmODg4MWNmOGFjZDI1N2FjYmEwOGRfckdGQW9XamZ4R0NEODc5bUdxelA3NlNIdDh2VWtpNHNfVG9rZW46Ym94Y243RWY4SEVSZ3ZTMVBvMkdMU3BWdnF4XzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MGEyMGJjYjQzMjBiZDQwMWVhNjk1ODIyNzExZDhhODBfQ0FFZWo4SER1THd6Z0xRaWVjck9panZZOFdDdUxZcUNfVG9rZW46Ym94Y25CUzFaM0VGcGp2dkE4VkdQSFM1amxnXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

#### 6.2.2进程原语(系统原生函数)

fork():进程创建

通过系统调用创建一个与原来进程几乎完全相同的进程,fork仅被调用一次，却能够返回两次，它可能有三种不同的返回值：1）在父进程中，fork返回新创建子进程的进程ID；2）在子进程中，fork返回0； 3）如果出现错误，fork返回一个负值；

https://blog.csdn.net/q1007729991/article/details/53728757

exec():进程重载

wait():进程回收

waitpid():进程回收

##### 6.2.1.1 fork()函数

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=ZjI2MjQ0YTE2NGE5ZWQ0ZjNmMWQ2YTg2NDI0MGY4MDZfYVA5YWtJOHRLeVlDTzQxT0E4bEVVNVc4SlhiWkZOemNfVG9rZW46Ym94Y244UTRxNjJxRmM0b2ZTWXhBUURSdlNjXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

父进程创建子进程

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MjBiZDNmZTVjOTY0NDExZjdiNzljMjQxODQ4YTdmMDRfUFFybTZpRlNHODRPaGQ4ZGxOVm1mUjVVQWE5UHg2NWxfVG9rZW46Ym94Y25Dc0hxVFlDTlJxWHIyUTN4b2J4STVmXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

注释：父进程执行任务从第一行代码执行到末尾

   子进程从fork之后执行到末尾

   父进程中的关键资源都会拷贝给子进程，但是修改子进程的资源，与父进程无关。

###### fork第二版：

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=ZGVmMzAxNzUyYWQ3NDI3MDdiY2Y2NGM4MzcwYTU1NTlfRkRlaFhRRGNOR2ZUdFh2OXFleGFZNHN3MHRYTXZ3RHVfVG9rZW46Ym94Y25VaTdtazRpckJiN0hiT1dxRk1kaDhnXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

###### fork第三版：

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=M2UxZmU2MjkwNWZmNGZmYjIxZTBlODM5YTdkZmM2Y2FfR1ZXRW9kVEt5dTE0aTNYSjNMdUxnY3U1SzQ4WGkyamNfVG9rZW46Ym94Y25WSFdFWExpVUJtbnM4UzZVVVpLejViXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

##### 6.2.1.2 exec()函数

##### 6.2.1.3 僵尸进程和wait()

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MGJhMWU4ODdlNWJhOTBmYmU2Njc2MjU5ZGVlNzEyODJfeEhJTEI0ZmcyUUw3ZXVOTVlaWXZJZHF4VEs2Q0drU2FfVG9rZW46Ym94Y25PV1hKbWszak4xMUpYVWZnSnBRWXRoXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

1.wait()函数当正确回收子进程时，返回一个大于0的进程ID，回收失败的时候返回0。

```C%2B%2B
pid_t wpid;
while((wpid = wait(NULL)) > 0){} // wait()回收子进程
```

​       采用阻塞wait()的方式回收子进程，会使得父进程无法完成自身核心工作，丧失工作能力。可将阻塞wait()回收改成waitpid()的非阻塞回收。

```C%2B%2B
pid_t wpid = waitpid(-1,NULL,WNOHANG); //参数pid是要回收的进程id      
```

wpid()的参数pid有4种，分别是 >0, 0, -1, <-1。pid > 0表示回收指定的进程；pid = 0表示回收当前调用进程同组的有权限回收的所有进程；pid = -1表示回收任意进程，只有进程是你的子进程就可以；pid<-1表示表示回收某一个组的进程，比如pid = -10000，表示回收在进程组10000中的所有子进程。

wpid()的返回值wpid有三种，分别是 >0, 0, -1。wpid > 0表示进程回收成功；wpid = 0表示轮询回收，比如某一个子进程正在运行无法回收，则进行回收下一个子进程，一会循环一遍之后，再来回收这个进程；wpid = -1表示当前父进程没有子进程，但是仍然调用wpid()会返回-1，表示没有可以回收的子进程，调用失败。

还有个参数option：WNOHANG

2.回收的最佳选择：

轮询回收可以穿插执行某一些工作，但也不是最合理的回收方法，因为给核心工作的资源过少。

最佳方法应该是：父进程执行自己的本职工作，没有干扰，当出现僵尸进程的时候，内核会给父进程发送SIGCHLD信号，父进程可以这个对信号进行捕捉，对子进程进行回收。

#### 6.3进程间通信

##### 6.3.1进程间通信的方式：socket        剪切板        内存映射        消息队列(POSIX SYSTEMV)        管道(有名管道 无名管道)        信号量        信号        条件变量        锁机制 临界区

https://blog.csdn.net/q1007729991/article/details/53996384

##### 6.3.2进程间通信的概念：使用某种技术实现进程之间的交互(数据交互、通知交互)

##### 6.3.3 进程如何通信？通信的数据保存在什么位置？

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=ZmNiNjU4ZTc3YzdjZTY1NGJmNzFhN2EyNzc5YTVlN2NfRDNzeW83cm1PZ1QzWlFHSzRBZm1wNXBGQkZlMGZLYlRfVG9rZW46Ym94Y25yV0ozWm5VTVk3eElLdnZtd3o1R0ljXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

##### 6.3.4.4管道(匿名管道和有名管道)

https://blog.csdn.net/weixin_38847128/article/details/83782217

###### 6.3.4.1匿名管道

1.匿名管道是在进程的内核空间创建一个缓冲区，这个缓冲区的数据可以被其他进程进行读写访问，实现多进程交互数据。

​    

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=NDcwYTAxZjdhNzdmNTZhMGM1NTJjYTkwZTY0ZjU4MmRfTFZzMG45ZUtaSXZzUUZ0a0wxTTg4MUU5VE1GazRaYUlfVG9rZW46Ym94Y25nd3pnelNWSlViOFM4ZFlteDVqWGRjXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

缓冲区大小是4K， Ubuntu16.04之前是64K。

2.管道具有流通性和方向性，因此在使用的时候，在使用管道的时候首先要确定通信方向。

管道创建函数：pipe(fds[2])                fds[0]：这是读端  fds[1]：这是写端

​    

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=YjQyMTE0ODc3MTJkYTI1ZWU4MGZiZGU5ZWYyZGJjNzFfUmFsaFBGalROWnEySHQzeU1IVnRjclRjUXZhaktsUGVfVG9rZW46Ym94Y25DZGtMSTF2M1k4eWplY3U0U3Ria2xaXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

子进程可以继承父进程的文件描述符，如果父进程有管道读写方式，那么也会被子进程继承，子进程可以访问读写管道。匿名管道仅支持父子间的通信。

3.数据通信方式：

单工通信（匿名管道就是典型的单工通信）

半双工通信（有名管道）

全双工通信（socket）

4.管道使用的特殊情况：

管道写满之后，写端继续写则堵塞

管道为空，读端继续读管道则堵塞

写端终止，读端读管道返回0（相当于读到末尾）

读端终止，写端写管道，内核向写端发送SIGPIPE信号，终止写端。

5.匿名管道的优缺点：

优点：实现简单，开发成本低

缺点：只能用于父子进程的通信，如果两个进程没有任何血缘关系，则匿名管道则不适用

匿名管道为单工通信，通信方向不能改变，通信不够灵活

匿名管道传递的是无格式字节流，不利于数据交互。

###### 6.3.4.2有名管道

1.创建有名管道的两种方式：

命令：mkfifo 有名管道名

函数：mkfifo("管道名"，权限(如0664))

​    

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=Mzg5YTc0YTM3OTk1ZTAyNmQwN2U2MWNhYmRhZWFmZjFfMklsY015bE1IMDk1M2tkVnhjSVdQdzZMUGVSY1FGaW9fVG9rZW46Ym94Y244S3YwaXVDamJvaVBpVVBmV1VNcU5oXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

2.注意事项：

有名管道是通过访问文件的方式访问管道缓冲区(没有任何访问限制)

使用管道文件读写管道的时候与管道文件无关，读写的数据会直接操作内核缓冲区，管道文件没有存储能力

访问有名管道文件的时候，必须满足读写权限才可以访问，否则open函数堵塞，堵塞到满足条件为止。

3.使用管道的时候要注意数据量：

根据数据量的多少有两种方式：原子访问(写入数据小于管道的大小)和非原子访问(写入数据大于管道的大小)。如果写入的数据量大于可用容量则挂起写进程。

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=Y2RmYTgzNTkwYjRiMWY3ODMyNjA0Mzk5NWM0N2ExNDlfa2VTYjZhRHBReFk5OEw3MkoyMzd0TEJYS1lFU3YzZFVfVG9rZW46Ym94Y25tSFp5a3h3dnBFeHV4UVVLcDNIclZoXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

##### 6.4.4.5内存共享映射

1.使用内存映射要导入头文件 #include

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MTMwYTc3MGUzYmIyY2VhNzVlM2M5ZjU1Mzc5MzFkZGRfeE5nQmdzTFBzclZLTEFhRlRnY3RkQUluZFEwaWc1STNfVG9rZW46Ym94Y25sMDBOWnI4dDFYMjQ4N3R2T1BBOU5oXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

2.内存映射的函数：void *p = mmap(NULL, 文件大小>0, PROT_READ|PROT_WRITE, MAP_SHARED(共享方式), fd, 0 );

释放映射的函数：munmap(p, 文件大小size);

#### 6.4进程关系

进程的关系主要有三种：亲缘关系  组关系  会话关系

##### 6.4.1孤儿进程

父进程先于子进程终止， 子进程失控(某些开发环境下， 孤儿进程的威胁远远大于僵尸进程)。init()进程

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=NjY3Y2ZkZGI0ODQxNjA5N2M5NDg4YTM0NGJmMDFmMDlfMmpjVzdHZ2tuUXM4M3pxODR5aFk5bzU1cGRwS0Vjc0NfVG9rZW46Ym94Y244OE1EOTZ0NlplWFVMVUo0MFFYbFE4XzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

##### 6.4.2精灵进程/守护进程(人为制造的孤儿进程)

###### 1.精灵进程的特点：

精灵进程的生命周期长(随系统持续，开机启动，关机结束)

工作周期长，周期工作，保证主程序核心稳定，正常工作。

###### 2.精灵进程的创建流程：

- 创建子进程 父进程退出

- 子进程创建新的回话，脱离原有的控制终端(比如脱离bash)

- 关闭无用的文件描述符

close(STDOUT_FILENO); //标准的输出

close(STDIN_FILENO); //标准的输入

- 改变进程工作目录， 改变到./ (防止文件被占用) chdir()

- 改变进程的umask掩码(防止文件权限混乱)

mode 0666  ~& 0002   得到0664

- 守护进程的核心工作

- 对守护进程的退出处理(释放资源，回收)

- 开机自动启动(shell脚本)

##### 6.4.3进程组和会话

https://www.cnblogs.com/liqiuhao/p/8087539.html

1.进程组的生命周期：进程组是否销毁与组长进程无关，当最后一个进程退出、终止或转移到其他组的时候，内核会销毁这个进程组。进程组的组长和组员没有必然的联系(不一定是亲缘关系)。

进程组的组长的getpid() 等于进程组的getgrep()。

###### 2.相关方法

```C%2B%2B
 getpgid(id)  //获取当前进程组的ID id是当前进程的id
getgrep()    //获取当前进程组的ID 不需要参数
setpgid()    //设置进程组的ID 可以使用这个方法对进程转移组 
//但是进程组的组长不可以转移进程组
```

###### 3.sesion会话

会话发起进程的进程ID: pid == pgid == sid

一个会话的结束会影响会话的参与者，可以采用创建新的会话的方法，让进程脱离控制终端。只有组员才可以通过setid进行新的会话的创建。

```C%2B%2B
getsid();  // 获取会话ID
setsid(setpgid());  //设置会话ID set
```

#### 4.信号(SIGNAL)

##### 4.1 信号

1.kill -l 查看进程所有支持的信号，kill是向某个进程发送一个通知或信号，绝大多数的信号都可以杀死一个进程。

2.Ubuntu16.04支持64个信号，1-31个信号被称为Unix经典信号(软件开发者经常使用)，34-64被称为自定义信号或者实时信号(底层开发者使用)。

###### 3.产生信号的几种方式：

终端组合按键产生信号 CTRL+C(2/SIGINT)， CTRL+\(3/SIGQUIT)，CTRL+Z(20/TSTP)

命令产生信号 kill +信号编号 进程ID

系统函数产生信号        kill(pid_t pid, int signo)         raise(int signo)        abrot(void)/SIGABRT

硬件异常产生信号        段错误(11/SIGSEVG)        浮点数例外(8/SIGFPE)        总线错误(调度异常)       

软条件产生信号        定时器到时接收信号(SIGALRM)        内核使用SIGPIPE杀死管道写端

###### 4. 信号的三大行为五种动作：

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MDE2ZjRhOWVjZjQwYjFhY2ZiNGQ0YjdjZjI5Zjk2ZjNfM0xFUXZNanFHVHlmMWRTVWVnU0JlRnNsS3NGSEJUTFJfVG9rZW46Ym94Y25ibUtyc2JhNW9HTFpiQUdKRDBkY1BjXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

###### 5.使信号失去作用的几种方式：

1.忽略信号                2.屏蔽阻塞信号                3.捕捉信号

###### 6.信号传递过程：

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=OWMwNDUwN2UzZjEzM2JjMDhhZTUzODhmZTVhMjc1ZDlfbDZsd0ZXYVJScFE2MFhkc0hGalk3VGNxRzlidldOYlhfVG9rZW46Ym94Y25zd0w0T0JsS0ZCcFNIN2RTWUhuSml3XzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

左侧文字内容：

未决信号集，未决态， 没有处理

抵达态 处理过的信号 信号抵达Handler

屏蔽阻塞一个信号，可以暂缓信号处理，但是信号并没有消失 比如信号2中的sigint

未决信号集内核设置更改，用户只要读的权限

阻塞信号集用户自定义设置， 用来阻塞某一个信号

经典信号不支持排队(最多支持一次排队，信号来了，屏蔽字置为1，如果handler时间过长，handler结束后，信号还会到达)/自定义信号支持排队（经典信号不支持排队，因为信号过了未决信号集，未决信号集就置为1，再来新的信号就会被丢弃）

阻塞信号集和未决信号集：https://blog.csdn.net/q1007729991/article/details/53886947

备注(信号集中的处理流程)：

内核空间的PCB中有两个信号集：未决信号集合，阻塞信号集。两个信号集中，0表示可以通过，1表示不能通过。信号穿过信号集的对应位置之后，对应位置置为1，再有相同的信号过来，直接被丢弃。当信号到达Handler后，未决信号集会重新置为0。

有两个信号直接为内核服务，这两个信号是9/SIGKILL和19/SIGSTP。这两个信号无法阻塞忽略和捕捉，只要发出必然杀死，只要发出必然挂起。

###### 7.使用系统API对信号进行阻塞：

sigset_t 信号集类型

```C%2B%2B
int sigemptyset(sigset_t *set);//将信号集初始化为0
int sigfillset(sigset_t *set);//将信号集初始化为1
int sigaddset(sigset_t *set, int signum); //将信号集某一个信号位置为1
int sigdelset(sigset_t *set, int signum); //将信号集的某一个信号位置为0
int sigismember(const sigset_t *set, int signum); //判断信号集的某一位是0还是1
sigprocmask(设置(SIG_SETMASK)，新的信号集,旧的信号集);//替换进程阻塞信号集
int sigaction(int signum, const struct sigaction *act, struct sigaction *oldact);
```

###### 8.信号行为变更：

https://blog.csdn.net/q1007729991/article/details/53887825

信号动作结构体：struct sigaction act

结构体中有不同的对象，分别有不同的作用：

act.sa_handler = SIG_DFL                SIG_IGN                （自定义行为）// 变更不同的行为

act.sa_mask = 信号集

act.sa_flags = 0

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=NzNiMDA1ZjM0MTdmNjFlY2VlYWMyNDMwMjlmMmZkNmVfU011UEh6MUJycXVacnlSU05aeUcybmxKSWVUUzRLQnlfVG9rZW46Ym94Y243YjFvWGVhUWV1eElFTTVKNzVBZ05kXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

###### 9.信号处理流程(内核处理信号)：

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MjIxMGFiOGNiYjBhZDk4MmE0ZDc0YTA5YzZjYTRhNmZfZHlFQnJzMU9kRWpsYUM1Z3BBVjNTdmVMNXJ3WEZHQ3FfVG9rZW46Ym94Y25HeG1POGVaZHlOZmFrakw3aWpVRHdiXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=Y2E5ZDk4MmFkNTRiZDUxNWQ3OWIxNDAwNDIzYzg2MGZfSGt3R3ZLV3dZQkt1U2hNRFFaNVl6cHVnbFJBb2hEcVhfVG9rZW46Ym94Y25FV3NJVTZuZGxuV3RNMWRHazU5blN0XzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MDFlNmIxZWQwNjQ2MzgxYTZkNjBhNDY0NWM1MWYzM2RfU2xoRk85T2FlcnJETUdtRzNzYmR6cjQyOXdqWTdwd0xfVG9rZW46Ym94Y25aaHRHWEJyM1Uxd1UwbEdOckJQOE1mXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

主控制流程和信号捕捉函数的冲突：

主控制流程和捕捉函数是串行执行的

主控制流程先执行，如果期间产生了信号，信号捕捉工作先做完，做完后返回主流程函数继续执行。

注意：信号捕捉流程尽量少使用静态变量和全局变量。

可重入函数与不可重入函数的区别：

函数内部使用全局资源被称为不可重入函数，但是信号处理的过程中严禁使用不可重入函数，会对系统程序造成隐患。

尽量使用可重入版本，如strtok_r        readdir_r都是可重入函数

###### 10.异步IO引起数据损坏(只会出现在32位的系统上)：

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MmY2OGFhY2QzZWIyYzYwNzBjOTU5ZTgyNmE0ZDg3MDVfQW1vT2dOeURSQXh2dk15MGFYcjRLb2hGV04wam1mOHZfVG9rZW46Ym94Y25LT3ZJZ251cnhLNDNwWEZKNHJBODFnXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=NTBkNjU0NDczYzc2ZWYzMzNkODE4YmIzMzFjMTkxZGRfV1d3VjNMa0RLcTh6UDgwaFJ6WUdCUDAxR2ZlQm9pSDVfVG9rZW46Ym94Y24zdkVURkwzN1NKYUxXYWNKM28zbTVjXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

long long 占8个字节,32位系统一次只能操作4个字节，对这两块内存处理的时候可能会出现数据损坏。处理这个问题要用原子类型，修饰符是sigatomac_t，可以检测处理位宽，保护数据。

#### 5.线程基础(THREAD)

高并发程序开发，相对于进程而言，线程模型体积更小，更加轻量，更加适合。进程包含线程，线程是进程分裂出来的新的调度单位。

##### 5.1 线程的创建：

1.线程创建的时候，内核要对线程资源进行初始化和分配内存。线程分裂的时候，会从进程中获取一部分的资源独占(从进程的堆空间中获取一部分空间用于创建线程的线程栈， 一般是8M的虚拟内存)。但是同时，进程和线程也会共享一部分资源。

线程控制块是TCB，线程的标志符tid是pthread_t类型的

###### 2.线程的共享资源和非共享资源

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=ZjM3Njk0NDQ3OGUwNDhiYTNmMjBlMmI2OGU2ODQxMjhfMGU2U2lEajJCNWNSQ1l4RFJzUHZ6RG1OS0NTTmxmMFhfVG9rZW46Ym94Y243VVkyczJLVk9oRnNQaGFuejloRW9FXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

CPU调度线程的过程：

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=YjZhYWZlNzdmMGYxMGI2ODI3NDgyOGY5Y2QyZjk1MWVfNFRiUEYzWUNIUG5Zb2J2ZmN2MmVmck1WQTdScVYxN3BfVG9rZW46Ym94Y25xWGhURWdKY084cFJPUW54c1Y4YnFjXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

只有分配到k，才能获取到CPU的时间片。进程可以退化成线程，如果进程独占资源的话，此时就是进程；但是如果某一个调度单位和进程共享资源，那么进程就会退化为主控线程，此时被创建出来的线程被称为普通线程。

##### 5.2 线程原语：

linux中使用的是第三方的线程库NPTL库。因此在编译的带线程的程序时候要加入库名 -lpthread，例如gcc a.c -lpthread -o app。使用NPTL要导入头文件#include

查看NPTL(Native POSIX Thread Library)的版本的命令：

```C%2B%2B
                getconf GUN_LIBPTHREAD_VERSION
```

###### 1.线程创建：

```C%2B%2B
int pthread_create(pthread_t tid, const pthread_attr_t *attr, void *(*pthread_job)(void *), void *arg);
```

四个参数分别是：传出的线程id， 线程属性(NULL是默认属性，一般设置为NULL)，线程工作函数，传给线程工作函数的参数。

在一个线程中调用pthread_create()创建新的线程后，当前线程从pthread_create()返回继续往下执行，而新的线程所执行的代码由我们传给pthread_create的函数指针pthread_job决定。pthread_job函数接收一个参数，是通过pthread_create的arg参数传递给它的，该参数的类型为void *，这个指针按什么类型解释由调用者自己定义。pthread_job的返回值类型也是void *，这个指针的含义同样由调用者自己定义。pthread_job返回时，这个线程就退出了，其它线程可以调用pthread_join得到pthread_job的返回值。

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=M2FmMWJkNWMyMjI2NWUzYTcyOTVkMDcxZGI1MjI4N2RfcGlNc2JZUXdaUVJJcWhTYkNoYllhNVl6QU5ScHFoRTlfVG9rZW46Ym94Y24wYzdUZXhjWEhvbktacHVjVm9XdmdoXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

###### 2.获取线程：

```C%2B%2B
                pthread_t pthread_self(void);//用来获取 线程的ID
```

###### 3.pthread_exit() 和 pthread_join()：

```C%2B%2B
                void pthreadexit(void *retval);
```

void *retval:线程退出时传递出的参数，可以是退出值或地址，如是地址时，不能是线程内部申请的局部地址。需要注意，pthread_exit或者return返回的指针所指向的内存单元必须是全局的或者是用malloc分配的，不能在线程函数的栈上分配，因为当其它线程得到这个返回指针时线程函数已经退出了。

调用线程退出函数，注意和exit()函数的区别，任何线程里的exit()都会导致进程退出，其他线程未工作结束，主控线程退出时不能return或exit。

###### 4.线程退出的方式：

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MjdmOWM1ODgxYWMyZTAwNDgzMDQzNDgzM2I3NzczYjJfdzlWeEpJVmlPU1NTMEw3MDdhS05HNWZVdFplWG9wNDJfVG9rZW46Ym94Y242TVFiNlNMMzhjbVhZa3MxYVZqaFViXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

```C%2B%2B
int pthread_join(pthread_t thread, void **retval); // 接收线程退出的时候 传出的值            
pthread_t thread:回收线程的tid；
void **retval:接收退出线程传递出的返回值，返回值：成功返回0，失败返回错误号。
```

 调用该函数的线程将挂起等待，直到id为thread的线程终止。thread线程以不同的方法终止，通过pthread_join得到的终止状态是不同的，总结如下：

如果thread线程通过return返回，retval所指向的单元里存放的是thread线程函数的返回值。

如果thread线程被别的线程调用pthread_cancel异常终止掉，retval所指向的单元里存放的是常数PTHREAD_CANCELED。

如果thread线程是自己调用pthread_exit终止的，retval所指向的单元存放的是传给pthread_exit的参数-1\\。

如果对thread线程的终止状态不感兴趣，可以传NULL给retval参数。

pthread_concel()

```C%2B%2B
                int pthread_cancel(pthread_t thread);
```

在进程内某个线程可以取消另一个线程。回收线程退出码的时候，只有线程是被cancel的时候，退出码才是-1，-1这个退出码保留不用。

###### 5.线程的两种回收状态：

线程的回收状态只有两种：回收态(JOABLE)                分离态(DETACH)

回收态是要靠自己进行回收，分离态是线程结束系统内核自动回收线程资源。

设置线程分离态的函数detach：

```C%2B%2B
int pthread_detach(pthread_t tid);
// 返回值：成功返回0， 失败返回错误码。
```

一般情况下，线程终止后，其终止状态一直保留到其它线程调用pthread_join获取它的状态为止。但是线程也可以被置为detach状态，这样的线程一旦终止就立刻回收。不能对一个已经处于detach状态的线程调用pthread_join，这样的调用将返回EINVAL。同样，对于回收状态的线程设置pthread_detach分离，也会设置失败。

优点：减少主控线程的负担

缺点：内核回收会将退出码销毁，主控线程无法获取，从而无法得知线程退出原因。

###### 6.隐藏函数入口和主函数

```C%2B%2B
 #include<stdio.h>
#include<stdlib.h>
// 隐藏主函数和函数入口　
//编译的时候命令是:
//gcc -nostartfiles -e 重命名 a.c -o app
 int whoami(){ //编译main函数的时候回生成一个_start的入口函数 可以不使用main而
 //使用_start作为函数的入口 编译的时候不生成_start函数 同时使用-e进行重命名 结束的时候
 //必须使用exit(0)进行回收资源。
     printf("HELLO WORLD\n");
     exit(0);
 }  
```

##### 5.3 线程属性：

###### 1.线程属性的结构体

```C%2B%2B
typedef struct
{
    int detachstate; //线程的分离状态
    int schedpolicy; //线程调度策略
    structsched_param schedparam; //线程的调度参数
    int inheritsched; //线程的继承性
    int scope; //线程的作用域
    size_t guardsize; //线程栈末尾的警戒缓冲区大小
    int stackaddr_set; //线程的栈设置
    void* stackaddr; //线程栈的位置
    size_t stacksize; //线程栈的大小
}pthread_attr_t;
```

属性值不能直接设置，须使用相关函数进行操作，初始化的函数为pthread_attr_init，这个函数必须在pthread_create函数之前调用。之后须用pthread_attr_destroy函数来释放资源。

###### 2.修改线程属性的作用：

改变线程调度的优先级

可以创建更多数目的线程

可以在创建线程之前就确定线程是分离态还是回收态

除了极其特殊的开发需求之外，不建议修改线程的属性，使用NULL作为默认的属性能满足开发中的大多数需求。

###### 3.初始化和销毁

```C%2B%2B
int pthread_attr_init(pthread_attr_t *attr); //初始化线程属性
int pthread_attr_destroy(pthread_attr_t *attr); //销毁线程属性所占用的资源
int pthread_attr_setdetachstate(pthread_attr_t *attr, int detachstate); //设置线程属性，分离or非分离
int pthread_attr_getdetachstate(pthread_attr_t *attr, int *detachstate); //获取程属性，分离or非分离

pthread_attr_t *attr:被已初始化的线程属性
int *detachstate:可选为PTHREAD_CREATE_DETACHED（分离线程）和PTHREAD _CREATE_JOINABLE（非分离线程）。
```

###### 4.线程栈

```C%2B%2B
int pthread_attr_setstackaddr(pthread_attr_t *attr, void *stackaddr);//设置线程栈的地址
int pthread_attr_getstackaddr(pthread_attr_t *attr, void **stackaddr);//获取线程栈的地址
int pthread_attr_setstacksize(pthread_attr_t *attr, size_t stacksize);//设置线程栈的大小
int pthread_attr_getstacksize(pthread_attr_t *attr, size_t *stacksize);//获取线程栈的大小
```

attr: 指向一个线程属性的指针

stackaddr: 返回获取的栈地址

返回值：若是成功返回0,否则返回错误的编号

创建线程的时候，每个线程从进程的堆空间划分资源(8M)用于创建用户栈。32位的系统下，0-3G的用户空间默认可以创建382个线程，增加线程的数量可以减少线程栈的大小。64位的操作系统对线程的数量有硬限制(10000多)，改变线程的属性不能改变线程的数量。可以自定义申请空间大小，当做线程栈来使用。

#### 5.4线程安全(线程同步)：

线程同步主要分为两部分：资源访问互斥和多线程协同工作

多线程是常见的并发程序开发模型，线程分用户级的线程和内核级的线程。

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MTdhOGM4YTVjOWZmNDc3MGFjZDBhODI4MDg0M2Q2ZTFfTU5FOVh6T0lwUzVGWjBqRFdQMWV2VUtFeDdRTDJac3RfVG9rZW46Ym94Y24yS0pIam13eFV2RmR6QTN3NEdSWk5lXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

###### 1.线程安全(保护共享资源)

多线程访问全局资源采用的互斥访问的方式，可以使用系统提供的锁技术完成线程对数据的互斥访问。多线程访问资源可能会导致结果异常，因为计算机的数据访问流程，两个线程如果同时访问某一个数据，可能会导致某线程执行操作失效。只对全局资源进行访问的时候才加锁。

使用互斥访问避免数据的锁技术主要有：

互斥锁        自旋锁        关键段        读写锁        线程锁        文件锁等

###### 2.互斥锁

互斥锁的基本操作：上锁和解锁，上锁就是创建一个临界区。

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDYyZjk2MThhMzFjZGQzYjQzNzlmMzhjZGYwN2IxMThfM2NqWmNZQ0VWclBCS1JKSzM2Sm03T0w3YXJuUjhncWhfVG9rZW46Ym94Y25OdDZaekFFZDJjend1dllkYkJiaFVoXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

由于临界区在同一个时间内只能由一个线程访问，当某一个线程结束访问的时候，其他的所有线程都会被唤醒，但是此时只能有一个线程获得访问临界区的权限，其余线程会继续被挂起，这样会引起大量无意义的系统消耗，就是所谓的惊群效应。

为了解决这个问题，linux内核创建一个等待队列，内核会随机对某一个线程发送唤醒标记，唤醒某一个线程。

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=NWQ2NTA4YWIzNTQzODNjYTM1YTQyMGY2YzI4NDNlOTVfYTBRZTFtSE9ScDNyNmNqd3A2Sm1lZUdwOUFCTENKMXlfVG9rZW46Ym94Y25sQ1RTUnMxY01oSVhIbGVOU2tLQjZkXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

3.互斥锁的API：

```C%2B%2B
 #include <pthread.h>
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER; /*创建互斥所变量并进行静态初始化*/
int pthread_mutex_destroy(pthread_mutex_t *mutex);     /*销毁锁*/
int pthread_mutex_init(pthread_mutex_t *restrict mutex, const pthread_mutexattr_t *restrict attr); /*动态初始化锁 第二个参数传NULL 采用默认属性*/
int pthread_mutex_lock(pthread_mutex_t *mutex);          /*为全局资源操作阻塞上锁*/
int pthread_mutex_trylock(pthread_mutex_t *mutex);      /*尝试拿锁，异步操作*/
int pthread_mutex_timelock(pthread_mutex_t *mutex); //定时堵塞获取锁
int pthread_mutex_unlock(pthread_mutex_t *mutex);       /*线程解锁，其他线程竞争*/
```

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=Y2Y5Y2I1YzIxZTkyMzdlNGQyZmMzNmJmYzcwN2M0ZDNfN211OG9idUtuWVN2ekV4UHRRbUo2ZkJmTkxPVXVRUXdfVG9rZW46Ym94Y25nanVodUhncHptR3FmY3NkSjhENEliXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

###### 3.读写锁

https://blog.csdn.net/q1007729991/article/details/61420757

读写锁：读时共享 写时独占。读写锁比互斥锁好点，因为读写锁允许多人同时多，能提高效率。

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=NzkxZjU1NmM3YTQ3Yjc0NTUzMjc0YjdkZjBhZTYxOGZfMVRTTnFEdTlVallySUpCaGlVYlJrODNjcE9aSnhBWkpfVG9rZW46Ym94Y24yd1pwUEozVTJTbGsxcHZjaWxWdkxmXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

写锁只有一个，读锁可以有多个。

```C%2B%2B
pthread_rwlock_t lock/*创建读写锁变量*/
pthread_rwlock_init();//读写锁 初始化
pthread_rwlock_destroy()// 读写锁销毁
pthread_rwlock_rdlock();//读锁定
pthread_rwlock_wrlock();//写锁定
pthread_rwlock_tryrdlock();//异步读锁定
pthread_rwlock_trywrlock();//异步写锁定
pthread_rwlock_unlock();//读写锁 解锁
```

###### 4.条件变量(多线程协同工作)

https://blog.csdn.net/q1007729991/article/details/61926511

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=NjJlOGQxOWE1MzAzZjBhYjJkNWQxYTU5MGFmZjY3MzRfbVlVREZSYlZTbDc5YUpHV1lGdUxuQTI4QnBNWE9yeVNfVG9rZW46Ym94Y25OQVZkTlFGUlptM2pNMWtNcWZBSkZoXzE2NDcxODIzNjA6MTY0NzE4NTk2MF9WNA)

```C%2B%2B
pthread_cond_t cond = PTHREAD_COND_INITIALIZER;/*创建环境变量并进行静态初始化*/
int pthread_cond_destroy(pthread_cond_t *cond);/*销毁环境变量*/
int pthread_cond_init(pthread_cond *restrict cond, const pthread_cond_t *restrict attr); /*动态初始化环境变量*/
int pthread_cond_wait(pthread_cond_t *cond,pthread_mutex_t *mutex);/*条件等待，第一次执行挂起线程，解锁，被唤醒执行，上锁*/
int pthread_cond_signal(pthread_cond *cond);/*向单个被挂机线程发送信号，使其唤醒*/
int pthread_cond_broadcast(pthread_mutex_t *cond);/*向所有被挂起线程发送信号，使唤醒
```

###### 5.进程锁

```C%2B%2B
int pthread_mutexattr_init(pthread_mutexattr_t *attr);
int pthread_mutexattr_setpshared(pthread_mutexattr_t *attr, int pshared);
int pthread_mutexattr_destroy(pthread_mutexattr_t *attr);
```

pshared:

线程锁：PTHREAD_PROCESS_PRIVATE

进程锁：PTHREAD_PROCESS_SHARED

默认情况是线程锁

###### 6.自旋锁

请求不到资源则自旋申请，直到获取到指定的资源。

###### 7.死锁

###### 8.哲学家就餐问题

https://blog.csdn.net/thelostlamb/article/details/80741319