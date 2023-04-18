Linux其他

# 1.正则表达式REGEX

正则表达式是由一组特殊符号组成的逻辑语句，用于描述数据的规则和特征。REGEX的主要作用就是匹配查找数据。REGEX利用的贪心算法，可以很快的匹配到相应的数据。

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MzMzNWJmOTA0NmY2NzI0NDAwZjY4MjE4ZDkyYTE4YmRfR2tjNm5rOE9zU1lRUjA3T29zZjRoU1FGSTE0VEhKSEZfVG9rZW46Ym94Y250SVdwRjV3a2JhOEpNTFMxdVFTV2llXzE2NDcxODIzNzc6MTY0NzE4NTk3N19WNA)

## 1.REGEX元字符

| 元字符   | 含义                                  |
| ----- | ----------------------------------- |
| *     | 以前一个字符为参照，表示参照字符出现0次或者多次            |
| .(点)  | 没有参照，单独出现表示任意字符1次                   |
| +     | 以前一个字符为参照，表示参照字符出现1次或多次             |
| ?     | 没有参照，单独出现表示任意字符出现0次或1次              |
| ^     | 以后一个字符为参照，表示行首                      |
| $     | 以前一个字符为参照，表示行尾                      |
| []    | 表示集合，a-z、A-Z、0-9表示集合范围内的任意字符        |
| [^]   | 表示对集合取非                             |
| {n}   | 以前一个字符为参照，表示出现的频率，n是出现n次            |
| {n,}  | 以前一个字符为参照，表示字符连续出现的频率，最少连续n次，最多无数次。 |
| {n,m} | 以前一个字符为参照，表示字符连续出现的频率，最少连续n次，最多m 次。 |
| \|    | 逻辑或                                 |
| ()    | 在regexec()中子正则要用括号括起来               |
| \     | 转义字符                                |
| +？    | 以前一个 字符为参照，表示参照字符出现1次或多次(非贪婪匹配)     |

​    正则表达式有两种工作模式，贪婪匹配和非贪婪匹配，使用+？可以对匹配模式进行切换，正则表达式最关键的是规则描述。

匹配手机号：

```C%2B%2B
                ^1[3-8][0-9]\{9\}$
```

### 1.REGEX函数(REGEX函数族)

```C%2B%2B
regcomp();//生成正则表达式
regexec();//用于在文本中匹配数据，抽取数据(给用户提供数据位置) 一次匹配一个数据 成功返回0
regfree();//释放正则占用内存
regerror();//不仅能反馈调用失败，还能反馈正则的语法错误
```

向数组中写入数据：

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=NzhjYWU3ZDFlOWJkNmJmODk1MjFlY2FlNTAxYjFlMjVfTWdGZUZEQU9yT0xNbnhWZURrNTVMc0REd1ZDSXdKU0hfVG9rZW46Ym94Y25hZUt0QkVJaGVCbHZBMHZvMmY0bmZnXzE2NDcxODIzNzc6MTY0NzE4NTk3N19WNA)

# 2.shell脚本

​     在linux和unix系统中，shell是个非常重要的概念和部件(命令解释器),用户在终端输入命令，shell对命令进行解释，操作系统执行功能。

## 1.shell执行过程

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=Y2FhYjdjNzZlNGUwYTJiZjIzMmE3NzhiNWRmNTQzOTBfdk9IMWp6akJTVlYzSVRnanh6bWVCcG5GeVY3Ym5WYXdfVG9rZW46Ym94Y24xR1E2aGxxY1Z2dE0wdHptdnZTU1hkXzE2NDcxODIzNzc6MTY0NzE4NTk3N19WNA)

Linux允许将若干命令集成到一个脚本文件中，然后执行脚本文件，批量使用命令。shell脚本提高了命令的可重用性。随着不断发展，允许在脚本中加入流程结构、逻辑判断、模块函数等，大大提高了脚本文件的灵活性，让命令集更有逻辑。

## 2.创建shell脚本

1.创建脚本文件，后缀是.sh

2.在脚本的第一行加入shell版本声明，#!/bin/bash

3.命令集合，逻辑处理

4.给脚本文件赋予执行权限 sudo chmod 0777 *.sh

3.shell脚本变量

- 临时变量：用户自定义的变量，一般变量名大写命名，定义并初始化变量，默认变量保存字符串，使用变量的时候使用$取变量。

- 永久变量：系统环境变量，随着系统持续，shell脚本中可以使用

- 位置遍历：$0-$9 一般第一个保留不用，用于接收命令行传参

- 特殊遍历：$*(所有参数)        $$(当前shell进程的pid)        $?(上一个命令的返回值)        $!(上一个执行后台命令的进程的pid)        $#(参数个数)

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MDE2MTBlNDk5ZGM5Yjc3NzgwNWZhMTNkOTNkZmNmYjVfRkVER1l4MDlMWlRGdXJERWtXR0U2SnQ5Z2lGQTVncHhfVG9rZW46Ym94Y25RYTh2d2lhZmhKOXhYTjF5YURJYkpjXzE2NDcxODIzNzc6MTY0NzE4NTk3N19WNA)

## 3.shell语句

```C%2B%2B
 echo $* #所有参数
echo $# #参数个数
echo $!    #上一个执行后台命令的进程的pid
echo $? #上一个命令的返回值
echo $$ #当前shell进程的pid
#三种修饰符 '' "" ``
CAR=奔驰
VAR='Hello '
 #双引号可以识别特殊符号做特殊处理,单引号中都为纯字符串
echo "${VAR}${CAR}"
#通过``修饰命令 执行命令并返回打印结果
#流程结构,if单选选择,case多项选择
if test -a $1
    then
        echo "文件存在"
    elif test 
        then
            if test
            fi
             echo "elif为真"
else             
    echo "文件不存在"
fi
 #while循环,以真为条件执行
while test 0
do
    echo runing....
done
#until循环,以假为条件执行
until test 1
do
  echo runing...
done
#函数内部使用位置变量需要在调用函数时传递, 用户自定义变量不需要,直接使用变量即可
```

# 3.爬虫

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=OGRjYTgwYjU2MjI1NTc2MWE2MTA0YmM2MTE1ODA3MDVfdVJnVTRBUURkN2JXOEhLNVRUOFNTVHVyR29WbEJ4djFfVG9rZW46Ym94Y25BRkNzQ2VMRGxOTTJvN09hRjhxZDF6XzE2NDcxODIzNzc6MTY0NzE4NTk3N19WNA)

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=YmNmODlmZTRiYmU5YmE0OTNkODM3NTM3NjFhYmMyZDRfQVU2cDBORlhiZnJaUUZ1ekxRQ1ZyNWhWUkVtWkRJR1dfVG9rZW46Ym94Y25yb2RTQUdWMHRkb29NcFVoNGZZVlJlXzE2NDcxODIzNzc6MTY0NzE4NTk3N19WNA)

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MDc3MmE2NmJkODM2OTcxNjE5ZmM0M2ZjOGRmNTQ1OTdfSVFmRHlTQ2J3YmpuU1MyM1FYRWlHVUhadmhhUnF1MWVfVG9rZW46Ym94Y25rbm5MQWh4OURsbGJRNHFGeGdDV1FoXzE2NDcxODIzNzc6MTY0NzE4NTk3N19WNA)

# 4.http协议基本交互

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MGUzY2UwN2Q0NGQ1Y2VmZTc3MTYzODNlNjIzMGY2ZDlfOTdkYVZSZ3lYMVJjaEtBYkZsRnl5WHJuMTZTQ1FMOVhfVG9rZW46Ym94Y25FbGlPM256N0pwYVBCV1F5N3ZtNzNnXzE2NDcxODIzNzc6MTY0NzE4NTk3N19WNA)

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=YjUzZDQyM2NhNTkyNGY5YTljMjZlYTFjM2YyYjkyMzJfSkpBczVpdDZyQk9HbkhZcnk2QkxyRG42T005TjhGcnVfVG9rZW46Ym94Y24xQXc2c2JldVppcVVkZXpOdWRHVnhoXzE2NDcxODIzNzc6MTY0NzE4NTk3N19WNA)

# 5.https协议基本交互

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MWIyZDIxNzkwZmI1YzIyN2FhNGMwMGI0YmIwOWNiNDNfNHhaakxKcnQ1c0tqeXlrakRJWmdmNDNYTHBTRG1HQnBfVG9rZW46Ym94Y25aYnB6YklHcFJVT1ZLVzdHSTR6eDhnXzE2NDcxODIzNzc6MTY0NzE4NTk3N19WNA)

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=YzQ5ZjQ5YTZhMjgyZDEwYjZiMDYzYmEzNTgzNTdhNDJfOVlXeXNLdXdPVFBod1A4dEo3d2lqRXVoYkpMVG1oUDVfVG9rZW46Ym94Y25uNGtCdmc5TVBUbEtkYlRlY1F4bUdkXzE2NDcxODIzNzc6MTY0NzE4NTk3N19WNA)