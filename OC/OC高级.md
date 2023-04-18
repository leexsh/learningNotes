IOS高级

# 多线程

1. 多线程几种方式

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MmEzNWJhOGZkNjE4MDg2ZmE1NWQxM2NkZGY2ZjU2YjhfNUV5TUNFV1J3dzY2emY2bm5acGF1WXF5NUFVdnprdDFfVG9rZW46Ym94Y243ZlhrMzF6UG50TVd6NDV1VnFLY1pjXzE2NDcxODI0MjY6MTY0NzE4NjAyNl9WNA)

线程属性：线程名称、线程优先级

## Pthread

```C%2B%2B
// 头文件
#import <pthread.h>
// 创建线程
// 1.线程编号地址、
// 2.线程属性
// 3.执行函数地址
// 4.函数参数
pthread_create($1, $2, $3, $4);
```

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=ZGI2YmQxZDVmNWFmMzMyMzMyYzJiNmU3NmY0ZjAzOWJfdlZsVk5QMHVzRG9YZEc1djJkWDVLUTdTdTM4eERRTm1fVG9rZW46Ym94Y25JQXpXZDdTNWNIOExYTXhyTzF2RndlXzE2NDcxODI0MjY6MTY0NzE4NjAyNl9WNA)

oc中的对象要传给C语言的函数时候，需要桥接(__bridge)

## NSThread

```Objective-C
NSThread *thread = [[NSThread alloc] init];
NSThread *thread = [[NSThread alloc] initWithTarget:(nonnull id) selector:(nonnull SEL) object:(nullable id);
[[thread start];
[[NSThread detacNewThreadSelector:@selector(SEL) toTarget:withObject：
```

## GCD

## NSOperation

## 多线程间的资源共享