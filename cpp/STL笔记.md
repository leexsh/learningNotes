STL笔记(源码剖析)

# 1.STL基本概念

STL(Standard Template Library,标准模板库)，是惠普实验室开发的一系列软件的统称。STL 从广义上分为: 容器(container) 算法(algorithm) 迭代器(iterator),容器和算法之间通过迭代器进行无缝连接。

STL提供了六大组件，彼此之间可以组合套用，这六大组件分别是:容器、算法、迭代器、仿函数、适配器（配接器）、空间配置器。

**容器：**各种数据结构，如vector、list、deque、set、map等,用来存放数据，从实现角度来看，STL容器是一种class template。

**算法：**各种常用的算法，如sort、find、copy、for_each。从实现的角度来看，STL算法是一种function tempalte。

**迭代器：**扮演了容器与算法之间的胶合剂，共有五种类型，从实现角度来看，迭代器是一种将operator* , operator-> , operator++,operator--等指针相关操作予以重载的class template. 所有STL容器都附带有自己专属的迭代器，只有容器的设计者才知道如何遍历自己的元素。原生指针(native pointer)也是一种迭代器。

**仿函数：**行为类似函数，可作为算法的某种策略。从实现角度来看，仿函数是一种重载了operator()的class 或者class template。

**适配器：**一种用来修饰容器或者仿函数或迭代器接口的东西。如STL提供的queue、stack等，虽然看着像容器，但是容器适配器，他们底层都是利用的deque。

**空间配置器：**负责空间的配置与管理。从实现角度看，配置器是一个实现了动态空间配置、空间管理、空间释放的class tempalte。

STL 具有高可重用性，高性能，高移植性，跨平台的优点。

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=ODE1NWQyNDM0NTViZjZhMTk0Y2UwMmY4ZDhkNGE3MmZfTkVyekxIRjExYVZFekVQMDVVWU5KRExOcmlwNmVyYkJfVG9rZW46Ym94Y25Ub3QxWFBXRFJWV1ZjNkg3U0pXZkVmXzE2NDcxODIzMzA6MTY0NzE4NTkzMF9WNA)

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=NWY0ZDA4YjQyY2M3NGJkYzIzNjM4NDU0ZWIzZTVhNGVfQWtodDQ1MnpaQmlaRkVyNW9ld1BVUzd5T2pOYnVBc1RfVG9rZW46Ym94Y253UU5XSGVvYVNCWFhVOUhpU0p6UXdnXzE2NDcxODIzMzA6MTY0NzE4NTkzMF9WNA)

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTViZjA4ZDI2N2YwZjY5ZGNiNzRkNDdkNmI4YTQ2YjhfM3hCbk9pUXRXampSc2ZKM01WaUx2bFRPVXYyOGpiYllfVG9rZW46Ym94Y25mWDc2SXZlU1JPNG92OHVkSVZscHdnXzE2NDcxODIzMzA6MTY0NzE4NTkzMF9WNA)

```C%2B%2B
 //这段代码可以将链表中内容拷贝到容器中去
 list<string> s = {"RNG", "IG","WE"};
 vector<string> vc;
 vc.assign(s.begin(), s.end());
```

使用[]和使用.at()访问容器中的元素的区别：

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=Yjc2NGZiMzQ5MjI4MjAxYTRiOWVhNDE3ZjFmY2U5NGNfNHZqMHN6dGVEUzhmQm9pN0l6Y3ZvR2R2RUhzbFJVaDZfVG9rZW46Ym94Y25sM2hlTmh1dGNocU1YM2g0OGZrUFcxXzE2NDcxODIzMzA6MTY0NzE4NTkzMF9WNA)

顺序容器的删除操作：

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=OGI0MTEwNTgxNmUzOGQ5MWQ0OWJlM2YyMzRiZTJjZTZfUjVKRVpCaWFFamNlZGZHcDhETFpkbFY5NGNZbTROdnBfVG9rZW46Ym94Y25icWlXTDgzT243V29EZDZrMnpLRTNnXzE2NDcxODIzMzA6MTY0NzE4NTkzMF9WNA)

# 2.vector容器

vector的数据安排以及操作方式，与array非常相似，两者的唯一差别在于空间的运用的灵活性。Array是静态空间，一旦配置了就不能改变，要换大一点或者小一点的空间，可以，一切琐碎得由自己来，首先配置一块新的空间，然后将旧空间的数据搬往新空间，再释放原来的空间。Vector是动态空间，随着元素的加入，它的内部机制会自动扩充空间以容纳新元素。

```C%2B%2B
//重新指定容器的长度为num，若容器变长，
//则以默认值填充新位置。如果容器变短，则末尾超出容器长度的元素被删除。
resize(int num);
//重新指定容器的长度为num，若容器变长，
//则以elem值填充新位置。如果容器变短，则末尾超出容器长>度的元素被删除。
resize(int num, elem);
capacity();//容器的容量
//容器预留len个元素长度，预留位置不初始化，元素不可访问。
reserve(int len);
//重新指定容器的长度为num，若容器变长，则以elem值填充新位置。
//如果容器变短，则末尾超出容器长>度的元素被删除。
resize(int num, elem);
capacity();//容器的容量
reserve(int len);//容器预留len个元素长度，预留位置不初始化，元素不可访问。
```

# 3.deque容器

## 1. 特性

Vector容器是单向开口的连续内存空间，deque则是一种双向开口的连续线性空间。所谓的双向开口，意思是可以在头尾两端分别做元素的插入和删除操作。

Deque容器和vector容器最大的差异，一在于deque允许使用常数项时间对头端进行元素的插入和删除操作。二在于deque没有容量的概念，因为它是动态的以分段连续空间组合而成，随时可以增加一段新的空间并链接起来。

## 2.API

```C%2B%2B
push_back(elem);//在容器尾部添加一个数据
push_front(elem);//在容器头部插入一个数据
pop_back();//删除容器最后一个数据
pop_front();//删除容器第一个数据
```

# 4.stack容器

```C%2B%2B
push(elem);//向栈顶添加元素
pop();//从栈顶移除第一个元素
top();//返回栈顶元素
empty();//判断堆栈是否为空
size();//返回堆栈的大小
```

# 5.queue容器

```C%2B%2B
push(elem);//往队尾添加元素
pop();//从队头移除第一个元素
back();//返回最后一个元素
front();//返回第一个元素
empty();//判断队列是否为空
size();//返回队列的大小
```

# 6.list(双向链表)

## 1.list容器不仅是一个双向链表，而且还是一个循环的双向链表。

## 2.常用API

```C%2B%2B
push_back(elem);//在容器尾部加入一个元素
pop_back();//删除容器中最后一个元素
push_front(elem);//在容器开头插入一个元素
pop_front();//从容器开头移除第一个元素
insert(pos,elem);//在pos位置插elem元素的拷贝，返回新数据的位置。
insert(pos,n,elem);//在pos位置插入n个elem数据，无返回值。
insert(pos,beg,end);//在pos位置插入[beg,end)区间的数据，无返回值。
clear();//移除容器的所有数据
erase(beg,end);//删除[beg,end)区间的数据，返回下一个数据的位置。
erase(pos);//删除pos位置的数据，返回下一个数据的位置。
remove(elem);//删除容器中所有与elem值匹配的元素。
size();//返回容器中元素的个数
empty();//判断容器是否为空
assign(beg, end);//将[beg, end)区间中的数据拷贝赋值给本身。
assign(n, elem);//将n个elem拷贝赋值给本身。
swap(lst);//将lst与本身的元素互换。
front();//返回第一个元素。
back();//返回最后一个元素。
reverse();//反转链表，比如lst包含1,3,5元素，运行此方法后，lst就包含5,3,1元素。
sort(); //list排序
```

# 7.set和multiset

## 1.特性

Set的特性是所有元素都会根据元素的键值自动被排序，因此set是有序且无重复的。multiset特性及用法和set完全相同，唯一的差别在于它允许键值重复，因此mulityset是有序而且允许重复的。set和multiset的底层实现是红黑树。

set容器的迭代器是const修饰的。可以利用迭代器来读取set容器中元素的值，但是不能修改。

## 2.常用API

```C%2B%2B
set<T> st;//set默认构造函数：
mulitset<T> mst; //multiset默认构造函数:
swap(st);//交换两个集合容器
size();//返回容器中元素的数目
empty();//判断容器是否为空
find(key);//查找键key是否存在,若存在，返回该键的元素的迭代器；若不存在，返回set.end();
count(key);//查找键key的元素个数
lower_bound(keyElem);//返回第一个key>=keyElem元素的迭代器。
upper_bound(keyElem);//返回第一个key>keyElem元素的迭代器。
equal_range(keyElem);//返回容器中key与keyElem相等的上下限的两个迭代器。
```

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=Mjk1Nzg5MmUyMjNhMmYyYjQ4MWNkYzZlMWM3NWI2MjlfWElYSHpweHNySVJmQ3U3Mm1CdXdjRlNqYXhUOTM1QVZfVG9rZW46Ym94Y25HSlFiZjBTeFRUQ293VlRGTjVUcktoXzE2NDcxODIzMzA6MTY0NzE4NTkzMF9WNA)

## 3.set中的迭代器

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MzBhZTUzNTA0MjNhMWMwODI2YzYwZGQ1YTA1ZmRiMTRfM0cyVzBCcHZzcEVSV1k1Mjg4b0I2WlZ4MVpoY2ZhWVpfVG9rZW46Ym94Y25ST3ZDOXBqdjNrdnNuUmttNWpZbVhlXzE2NDcxODIzMzA6MTY0NzE4NTkzMF9WNA)

# 8.对组pair

对组(pair)将一对值组合成一个值，这一对值可以具有不同的数据类型，两个值可以分别用pair的两个公有属性first和second访问。类模板：template 

```C%2B%2B
// pair的两种创建
pair<string,int> pa{"小明",18};
pair<string,int> pa = make_pair("小华",13);
```

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=OWYwZTAyYzg0ZjNkMTczNjg1ODI0ZWI2NGNmNmU3OTFfQnZVSEFVRG1GMHo0VkJhWFNZaFlVUjlzcjAza3o4OVlfVG9rZW46Ym94Y256RDR2djNFT3BReVYxbEVnazRmYjdmXzE2NDcxODIzMzA6MTY0NzE4NTkzMF9WNA)

# 8.map

## 1.特性

Map的特性是，所有元素都会根据元素的键值自动排序。Map所有的元素都是pair,同时拥有实值和键值，pair的第一元素被视为键值，第二元素被视为实值，map不允许两个元素有相同的键值。Multimap和map的操作类似，唯一区别multimap键值可重复。

对于map容器，他得first成员是const修饰的，不能修改，second成员是可以修改的。

```C%2B%2B
 // 第一种 通过pair的方式插入对象
mapStu.insert(pair<int, string>(3, "小张"));
// 第二种 通过pair的方式插入对象 推荐用法
mapStu.inset(make_pair(-1, "校长"));
// 第三种 通过value_type的方式插入对象
mapStu.insert(map<int, string>::value_type(1, "小李"));
clear();//删除所有元素
erase(pos);//删除pos迭代器所指的元素，返回下一个元素的迭代器。
erase(beg,end);//删除区间[beg,end)的所有元素 ，返回下一个元素的迭代器。
erase(keyElem);//删除容器中key为keyElem的对组。
find(key);//查找键key是否存在,若存在，返回该键的元素的迭代器；/若不存在，返回map.end();
count(keyElem);//返回容器中key为keyElem的对组个数。对map来说，要么是0，要么是1。对multimap来说，值可能大于1。
lower_bound(keyElem);//返回第一个key>=keyElem元素的迭代器。
upper_bound(keyElem);//返回第一个key>keyElem元素的迭代器。
equal_range(keyElem);//返回容器中key与keyElem相等的上下限的两个迭代器。
```

## 2.map的insert操作

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MzZlMzI4ZWViNzAxMDY4MmFhODcwYzA1MTkzMWMzYTlfSU5NQnN6V0VyYlJSc0pMNlNobVZNWWpOZjUzMHRCd1lfVG9rZW46Ym94Y240d0RzODY4aDB2dGEzcG5IMWtYZ2RjXzE2NDcxODIzMzA6MTY0NzE4NTkzMF9WNA)

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=N2NhY2ZmMDE0MmFkYmMyNGRhMDk0ZDBlODdmY2YxM2RfRm5KVVpEeEVxRUNMSEt6MlI4V0QwWlJEYW55eE85UFVfVG9rZW46Ym94Y25EbW91T3NEeEEzb0NZOWd3Q2dlMGpkXzE2NDcxODIzMzA6MTY0NzE4NTkzMF9WNA)

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MGZmNWEzNzQzMGQ2ZTI4MTIxNTcyYWEwMjYyNTE4OTlfOVpPZG5tcUFQV1NPYmtwYjVWb0ZFN09CTFpTT2tpZFFfVG9rZW46Ym94Y240NXFSV2h5aFF6SG1Kc1c4R1FIa3RlXzE2NDcxODIzMzA6MTY0NzE4NTkzMF9WNA)

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=YzMwOTA4YjYyOTM0MzU3ZWZhMWY5MTEyYzczMWYwMWJfcjdDaUJOTjhXSmtUc2NnVkVKY2JZeW9hVlFITkpoTG5fVG9rZW46Ym94Y24xT0lXTUdaTWtCdUpXQzQzbXJSVmtUXzE2NDcxODIzMzA6MTY0NzE4NTkzMF9WNA)

## 3.map的下标操作

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MDczNTEzOTk2Y2U5YTk4YmE4MDk2MmJhZTAyMDE1NGZfdDFCUVBDR2l1Y081UWVlcDRzZG8wVTRZSnE0eDF0azRfVG9rZW46Ym94Y244YVgxUGRXT1dTQmV3blBrNW00cFhmXzE2NDcxODIzMzA6MTY0NzE4NTkzMF9WNA)

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MzcyY2EzNWZhNGZhNmJkNjAyZDA4NDc5Zjk5MDc3NjBfMW92elF4MW5yeVVXREhNQldQaGlkNVJjbGNQTmViUXJfVG9rZW46Ym94Y25US0szTThiOWd6bVJQUGJHQnRjcGVnXzE2NDcxODIzMzA6MTY0NzE4NTkzMF9WNA)

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=YjUxYzBhNmZiODViNWU4OTA3Mjc0OTNhZmU2OGEyMTNfdjVsV0gyaFZsMzVyMnZ5Ym5hZlhzTEFOaEFreEtISlZfVG9rZW46Ym94Y25oa2tEeGVkVFBNM1RsQmg1dElSa01lXzE2NDcxODIzMzA6MTY0NzE4NTkzMF9WNA)

# 9.内建函数对象

STL内建了一些函数对象。分为:算数类函数对象,关系运算类函数对象，逻辑运算类仿函数。这些仿函数所产生的对象，用法和一般函数完全相同，当然我们还可以产生无名的临时对象来履行函数功能。使用内建函数对象，需要引入头文件 #include。

```C%2B%2B
template<class T> T plus<T>//加法仿函数
template<class T> T minus<T>//减法仿函数
template<class T> T multiplies<T>//乘法仿函数
template<class T> T divides<T>//除法仿函数
template<class T> T modulus<T>//取模仿函数
template<class T> T negate<T>//取反仿函数
template<class T> bool equal_to<T>//等于
template<class T> bool not_equal_to<T>//不等于
template<class T> bool greater<T>//大于
template<class T> bool greater_equal<T>//大于等于
template<class T> bool less<T>//小于
template<class T> bool less_equal<T>//小于等于
template<class T> bool logical_and<T>//逻辑与
template<class T> bool logical_or<T>//逻辑或
template<class T> bool logical_not<T>//逻辑非
```

# 10.适配器

c++11 适配器改版了 详细见 C++ primer P354

采用了占位符，需要打开命名空间placeholders。使用的bind()函数。

[c++11随记：std::bind及 std::placeholders](https://blog.csdn.net/weixin_43333380/article/details/82935291)

# 11.空间配置器

## 1.介绍

https://blog.csdn.net/qq_40421919/article/details/89192383

STL的操作对象（所有的数值）都存放在容器之内，而容器一定需要配置空间以置放资料。

- 标准的空间配置器：std::allocator，在SGI中，因为效率低，一般不建议使用。只是对基层内存配置/释放行为::operator new和::operator delete做了薄薄的包装

- 特殊的空间配置器：std:alloc

```C%2B%2B
                Foo* pf = new Foo; //配置内存，然后建构对象
delete pf;  //将对象解构，然后释放内存
```

​       这其中的  new 算式内含两阶段动作 ：(1)呼叫 ::operator new 配置内存，(2)呼叫 Foo::Foo() 建构对象内容。 delete 算式也内含两阶段动作： (1)呼叫Foo::~Foo() 将对象解构，(2)呼叫 ::operator delete 释放内存。、

为了精密分工，STL allocator 决定将这两阶段动作区分开来。内存配置动作由 alloc:allocate() 负责，内存释放动作由 alloc::deallocate() 负责；对象建构动作由 ::construct() 负责，对象解构动作由 ::destroy() 负责。

分别在

```C%2B%2B
#include <stl_alloc.h>//负责内存空间的配置与释放
#include <stl_construct.h>//负责对象内容的建构与解构
```

​       https://blog.csdn.net/u014209688/article/details/90047713 new (p) T1(value) ;的一个解释

## 2.空间配置器实现的原理：

​        考虑小型区块所可能造成的内存破碎问题，SGI 设计了双层级配置器，第一级配置器直接使用  malloc() 和 free() ，第二级配置器则视情况采用不同的策略：当配置区块超过128bytes，视之为「足够大」，便呼叫第一级配置器；当配置区块小于 128bytes，视之为「过小」，为了降低额外负担，便采用复杂的memory pool整理方式，而不再求助于第一级配置器。

memory pool(记忆池)：每次配置一大块内存，并维护对应之自由链表（free-list）。下次若再有相同大小的内存需求，就直接从free-lists中拨出。如果客端释放小额区块，就由配置器回收到free-lists中—是的，别忘了，配置器除了负责配置，也负责回收。为了方便管理，SGI第二级配置器会主动将任何小额区块的内存需求量上调至8的倍数（例如客端要求 30 bytes，就自动调整为 32bytes），并维护 16 个 free-lists，各自管理大小分别为 8, 16, 24, 32, 40, 48, 56, 64, 72,80, 88, 96, 104, 112, 120, 128 bytes的小额区块。

# 12.迭代器

- 迭代器是一种类似指针的东西。
- 迭代器对应的类别：

(1).value type：迭代器所指向对象的类别

(2).difference type：表示两个迭代器之间的距离，也可以表示一个容器的最大容量，也就是头尾距离。

(3).reference type：

(4).pointer type：

(5).iterator_category

# 13.常用的遍历算法

## 1.for_each()

```C%2B%2B
for_each(param beg, param end,param callback){
    return;
}
/*
    @param beg 开始迭代器
    @param end 结束迭代器
    @param callback  函数回调或者函数对象
    @return 函数对象
*/
```

## 2.transform();

注意：transform()不会给目标容器自动分配内存，需要我们提前分配好。

```C%2B%2B
transform算法 将指定容器区间元素搬运到另一容器中
transform(param beg1,param end1,param neg2,param callback){}
/*
    @param beg1 原容器开始迭代器
    @param end2 原容器结束迭代器
    @param beg2 拷贝容器开始迭代器
    @param callback  函数回调或者函数对象
*/
```

# 13.常用的查找算法

```C%2B%2B
find(iterator beg, iterator end, value)
/*
    查找自定义对象，需要在类中重载==运算符。
    @param beg 容器开始迭代器
    @param end 容器结束迭代器
    @param value 查找的元素
    @return 返回查找元素的位置的一个迭代器
*/
find_if(iterator beg, iterator end, _callback);
/*
    find_if算法 条件查找
    @param beg 容器开始迭代器
    @param end 容器结束迭代器
    @param  callback 回调函数或者谓词(返回bool类型的函数对象)
    @return bool 查找返回true 否则false
*/
adjacent_find(iterator beg, iterator end, _callback);
/*
    adjacent_find算法 查找相邻重复元素
    @param beg 容器开始迭代器
    @param end 容器结束迭代器
    @param  _callback 回调函数或者谓词(返回bool类型的函数对象)
    @return 返回相邻元素的第一个位置的迭代器
*/
bool binary_search(iterator beg, iterator end, value);
/*
    binary_search算法 二分查找法
    注意: 在无序序列中不可用
    @param beg 容器开始迭代器
    @param end 容器结束迭代器
    @param value 查找的元素
    @return bool 查找返回true 否则false
*/
count(iterator beg, iterator end, value);
/*
    count算法 统计元素出现次数
    @param beg 容器开始迭代器
    @param end 容器结束迭代器
    @param  value回调函数或者谓词(返回bool类型的函数对象)
    @return int返回元素个数
*/
count_if(iterator beg, iterator end, _callback);
/*
    count算法 统计元素出现次数
    @param beg 容器开始迭代器
    @param end 容器结束迭代器
    @param  callback 回调函数或者谓词(返回bool类型的函数对象)
    @return int返回元素个数
*/
```

# 14.常用的排序算法

```C%2B%2B
merge(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest)
/*
    merge算法 容器元素合并，并存储到另一容器中
    @param beg1 容器1开始迭代器
    @param end1 容器1结束迭代器
    @param beg2 容器2开始迭代器
    @param end2 容器2结束迭代器
    @param dest  目标容器开始迭代器
*/
sort(iterator beg, iterator end, _callback)
/*
    sort算法 容器元素排序
    注意:两个容器必须是有序的
    @param beg 容器1开始迭代器
    @param end 容器1结束迭代器
    @param _callback 回调函数或者谓词(返回bool类型的函数对象)
*/
reverse(iterator beg, iterator end)
/*
    reverse算法 反转指定范围的元素
    @param beg 容器开始迭代器
    @param end 容器结束迭代器
*/

15.常用的拷贝算法

                copy(iterator beg, iterator end, iterator dest)
/*
    copy算法 将容器内指定范围的元素拷贝到另一容器中
    @param beg 容器开始迭代器
    @param end 容器结束迭代器
    @param dest 目标起始迭代器
*/
replace(iterator beg, iterator end, oldvalue, newvalue)
/*
    replace算法 将容器内指定范围的旧元素修改为新元素
    @param beg 容器开始迭代器
    @param end 容器结束迭代器
    @param oldvalue 旧元素
    @param oldvalue 新元素
*/
replace_if(iterator beg, iterator end, _callback, newvalue)
/*
    replace_if算法 将容器内指定范围满足条件的元素替换为新元素
    @param beg 容器开始迭代器
    @param end 容器结束迭代器
    @param callback函数回调或者谓词(返回Bool类型的函数对象)
    @param oldvalue 新元素
*/
swap(container c1, container c2)
/*
    swap算法 互换两个容器的元素
    @param c1容器1
    @param c2容器2
*/
accumulate(iterator beg, iterator end, value)
/*
    accumulate算法 计算容器元素累计总和
    @param beg 容器开始迭代器
    @param end 容器结束迭代器
    @param value累加值
*/
fill(iterator beg, iterator end, value)
/*
    fill算法 向容器中添加元素
    @param beg 容器开始迭代器
    @param end 容器结束迭代器
    @param value t填充元素
*/
```

# 16.常用的集合算法

```C%2B%2B
set_intersection(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest)
/*
    set_intersection算法 求两个set集合的交集
    注意:两个集合必须是有序序列
    @param beg1 容器1开始迭代器
    @param end1 容器1结束迭代器
    @param beg2 容器2开始迭代器
    @param end2 容器2结束迭代器
    @param dest  目标容器开始迭代器
    @return 目标容器的最后一个元素的迭代器地址
*/
set_union(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest)
/*
    set_union算法 求两个set集合的并集
    注意:两个集合必须是有序序列
    @param beg1 容器1开始迭代器
    @param end1 容器1结束迭代器
    @param beg2 容器2开始迭代器
    @param end2 容器2结束迭代器
    @param dest  目标容器开始迭代器
    @return 目标容器的最后一个元素的迭代器地址
*/
set_difference(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest)
/*
    set_difference算法 求两个set集合的差集
    注意:两个集合必须是有序序列
    @param beg1 容器1开始迭代器
    @param end1 容器1结束迭代器
    @param beg2 容器2开始迭代器
    @param end2 容器2结束迭代器
    @param dest  目标容器开始迭代器
    @return 目标容器的最后一个元素的迭代器地址
*/
```

​                     