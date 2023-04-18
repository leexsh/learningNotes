iOS学习

1. # 基础

2. ## import指令
- \#import 是 #include的增强  无论import多少次  只会包含一次。
1. ## autoreleasepool:自动释放池

2. ## 编译 cc -c XX.m        链接 cc xx.o framework  框架名称   链接成功后即可执行out文件

3. ## OC的数据类型
- OC支持C的所有数据类型  除此之外还新增了一些类型， 
  
  - 例如BOOL类型  YES NO
  - Boolean类型 一般存储条件表达式的结果 成立为true  否则false 
  - id类型  万能指针
  - nil  跟NULL差不多
  - SEL  方法选择器
  - block 代码段
1. ## OC完全兼容C语言
- OC中可以写任意的C语言代码，OC 支持C的所有运算符  所有的控制语句  所有关键字  同时OC也新增了一些关键字 一般都是用@开头
1. ## 内存和对象

```Objective-C
Object *p = [Object new];
```

- 上述代码进行申请空间，并进行初始化；

- 类首次使用的时候，会进行类加载，将类加载到代码段；

- 没个对象中还有个isa指针，指向了代码段中类的位置，方便调用类的方法。
1. ## nil和NULL的区别
- ### NULL
  
  - NULL可以作为指针变量的值
  - NULL等价与0

- ### nil
  
  - 只能作为指针变量的值，代表指针变量不指向内存中的任何位置
  - nil也等价于0

所以 NULL等价于nil

- ### 使用建议
  
  - 一般，C的指针用NULL， oc的类指针用nil
1. ## 编程规范
- 类中属性名一定要用下划线开头，且不允许声明的时候初始化

- 类名一定大写
1. ## 异常处理
- ### 语法

```Objective-C
@try {

}
@catch(Exception *ex) {

}
@finally{

}
```

- 当try某行发生异常，不会继续执行try中的代码，直接转向执行catch的代码；
- catch中的代码只有发生异常的时候才会执行；
- catch中参数Exception可以通过%@打印出异常的值；
- finally都会被执行；
- 无法处理C语言的异常。
1. ## 类方法
- 声明
  - 使用 + 声明
- 调用
  - 不需要创建对象来调用，而是使用类名来调用；

```Objective-C
// 类方法调用
[类名 类方法];
```

- 特点
  
  - 节约空间 提高效率
1. ## 匿名对象
- 格式

```Objective-C
[类名 new];
```

- 匿名对象只能使用一次
1. ## static关键字
- 不能修饰属性和方法

- 可以修饰方法中的局部变量，修饰后，称为静态变量，存在常量区，下次执行会直接使用，而不会再声明
1. ## self  一个指针
- 可以用在对象方法和类方法中

- 在对象方法中，self指向当前对象；在类方法中，self指向当前类

- 调用对象/类的方法class，会返回类的地址，[对象 class]
1. ## 继承
- 单继承

- 传递性

- 所有类都继承于NSObject
1. ## super关键字
- 可以用在对象方法和类方法中

- 在对象方法中使用super可以调用从父类继承过来的方法

- 在类方法中，可以使用super调用当前类从父类中继承过来的方法
1. ## 访问修饰符 默认是@protected修饰
- @private 私有的  只能在本类中访问

- @protected 受保护的 只能在本类和本类的子类中访问

- @package 可以在当前框架中访问

- @public 公共的
1. ## 私有方法：只写实现 不写声明

2. ## 方法重写  子类重写父类的方法

3. ## 类
- 类是以class的形式存储在代码段中，可以通过对象或类的class方法得到类

```Objective-C
// c1就是对象o的类   可以通过c1调用该类的类方法
Class c1 = [对象o class]；
```

1. ## SEL
- 全称叫 selector选择器，是一个数据类型，也可以说是一个类，
- SEL对象是用来存储一个方法  在类中方法的信息存储在SEL对象中，再将sel对象作为类的属性
- 取到存储方法的SEL对象

```Objective-C
// 如果方法有参数 方法名写的时候要加：
SEL sel = @selector(方法名);
```

- 调用方法的本质

```Objective-C
// 1.先拿到存储say方法的SEL对象，即sel消息
// 2.把sel消息发送到c1对象
// 3.根据isa指针在类中找到与sel相对应的方法，如果有，就执行，如果没有就继续在父类中寻找
[c1 say];
```

- 想要通过SEL对象调用方法，则如下

```Objective-C
// c1对象通过performSelector方法调用sel对象
// performSelector方法原型： -(id)performSelector : (SEL)aSelector;
[c1 performSelector : sel对象];
```

- 可以根据SEL去判断类中是否有某个方法：

```Objective-C
// 函数原型： - (BOOL)respondsToSelector:(SEL)aSelector;
// 判断c1这个对象所在的类中是否有aaa这个方法，有的话返回TRUE 否则FALSE
BOOL b = [c1 respondToSelector : @selector(aaa)];
```

1. ## @property
- 自动生成get和set方法的声明 

- ### 使用方法

```Objective-C
 // 属性age的get和set方法的声明
 @property int age;
```

- ### property修饰参数
  
  - 与多线程有关的参数
    - atomic(默认值) 生成的set方法会有锁，是线程安全的，但效率低
    - nontomic 没有线程锁 不安全但是效率高
  - 与生成的set方法相关的参数
    - assign(默认值) 生成的set方法是直接赋值
    - retain 生成的setter方法是标准的MRC管理方式(所以只能用在MRC模式下)，先判断新旧对象是不是同一个对象，如果不是就先realse 再retain
    - 使用建议：

当属性类型是OC对象类型的时候，大多数使用retain修饰

当属性类型不是OC对象类型的时候，就可以使用assign修饰

- 与读写相关的参数
  - readonly 只生成get方法 不生成set方法
  - readwrite (默认值) 同时生成set和get方法
- 与生成的set和get方法相关的参数
  - getter
  - setter
- 强弱类型
  - strong 强类型(只能用在ARC模式下)
  - weak 弱类型 (循环引用的情况下可以考虑)
  - 使用建议：

ARC机制下，当属性类型是OC对象类型的时候，绝大多数使用strong修饰

ARC机制下，当属性类型不是OC对象类型的时候，绝大多数使用weak修饰

1. ## @synthesize
- 自动生成get和set方法的实现
- 使用方法：

```Objective-C
// 属性age的set和get方法实现  在implementation中
@synthesize age；
```

1. ## id指针和instancetype
- id指针是万能指针，类似于(void *)

- 如果方法的返回值是instancetype，那么方法返回的是当前类的对象

- ### 使用建议：
  
  - 如果方法是创建当前类的对象，不要写死成类名，而是用[类名 new];
  - 如果方法的返回值是当前对象，也不要写死，返回值类型应该是instancetype

- ### id和instancetype区别：
  
  - instancetype只能作为函数的返回值，id可以声明指针变量、可以作为参数和返回值
  - instancetype是有类型的，代表着当前类的对象，而id是个无类型的指针

```Objective-C
- (instancetype) init{
    if (self = [super init]) {

    }
    return self;
}
```

1. ## 动态类型检查
- 判断对象中是否有这个方法可以执行

```Objective-C
// 函数原型： - (BOOL)respondsToSelector:(SEL)aSelector;
BOOL b = [c1 respondToSelector : @selector(aaa)];
```

- 判断指定对象是否为指定类或子类的对象

```Objective-C
// 函数原型：- (BOOL)isKindOfClass:(Class)aClass;
```

- 判断指定对象是否为当前类的对象，不包括子类

```Objective-C
//- (BOOL)isMemberOfClass:(Class)aClass;
```

- 判断当前类是否为某个类的子类

```Objective-C
// + (BOOL)isSubclassOfClass:(Class)aClass;/+ (BOOL)isSubclassOfClass:(Class)aClass;
```

1. ## init方法和重写init方法
- [[class alloc] init]
- 可以重写init方法
  - 重写init方法，必须要先调用父类的init方法

```Objective-C
self = [super init];
```

- 调用init失败  失败的话 就会返回nil, 因此要判断父类是否初始化成功

```Objective-C
if (self = [super init]) {
    // 执行操作
}
```

- 注意
  
  - 自定义init方法返回值必须是instancetype
  - 自定义init方法的方法名最好用initWith
1. # 内存管理和其他

2. ## 引用计数器
- 每个对象都有一个retainCount属性，叫做引用计数器，用来计算有多少人在使用

- 当引用计数器为0的时候，会进行回收

- 每为对象发送一次retain消息，就会使得引用计数器+1

- 当为对象发送一次release时，引用计数器就会-1 而不是直接回收对象，而是当引用计数为0的时候被回收

- 当对象被回收的时候，就会调用dealloc方法
1. ## 内存管理的分类
- ARC(自动内存管理)

- MRC(手动内存管理)
  
  - 使用MRC的时候，在进行dealloc的时候，一定要在最后一行调用[super dealloc]
1. ## 内存管理的原则
- 什么时候发送retain消息
  
  - 当多一个人使用的时候，先为这个对象发送retain消息

- 什么时候发送release消息
  
  - 当少一个人使用的时候，要先发送release消息

- 内存管理原则
  
  - 有对象创建，就要有release
  - retain和release次数要匹配
  - 谁要用 谁就retain
  - 谁不用 谁叫release
1. ## MRC相关操作
- MRC下set方法的形式

```Objective-C
// 例子 Car的set方法
- (void) setCar : (Car*) car
{
    if (_car != car) {
        [car release];
        _car = [car retain];

    }
}
```

- 还要重写dealloc方法

```Objective-C
- （void）dealloc{
    // ...
    [_car release];
    [super dealloc];
}
```

1. ## @class 类的声明 避免循环引用

2. ## 自动释放池
- 存入到自动释放池的对象，在自动释放池被销毁的时候，会自动调用释放池中所有对象的release方法，自动释放对象
- 自动释放池的创建

```Objective-C
@autoreleasepool {
    // 自动释放池的范围
}
```

- 将对象加入到自动释放池

```Objective-C
// 将Person对象p加入到自动释放池
@autoreleasepool{
    Person *p = [[[Person alloc] init] autorelease];
}
```

- 注意事项
  - 只有调用对象的autorelease方法后，才会把对象加入到自动释放池
  - 对象的创建可以放在自动释放池之外，但是要在自动释放池中调用autorelease方法
  - 自动释放池介绍的时候，只是向对象发送release消息，并不是销毁对象
  - 对一个对象多次调用autorelease， 在结束的时候，会对对象发送多次autorelease消息
  - 自动释放池可以嵌套 
  - 将对象放到自动释放池中，并不会使得引用计数器+1
  - 
1. ## ARC(自动内存管理 automatic reference counting)
- ARC是编译机制，在编译的时候编译器会在合适的位置加入retain/release/autorelease
- ARC下对象何时被释放

只要没有强指针指向对象就会被回收，即引用计数为0

- 强指针和弱指针
  - 默认一个指针就是强指针
  - 也可以用_strong来显式声明强指针和弱指针

```Objective-C
__strong Person *p = [Person new];
__weak Person *p = [Person new];
```

- 区别

强弱指针 本质没啥区别  只是ARC模式下回收的基准

1. ## 非正式协议
- 为系统自带的类写分类 

- 为存在的类添加方法
1. ## 分类(category)

```Objective-C
@interface LiveCore(Interact)
@end
```

- 分类使用注意事项
  - 分类中只能加方法 不能加属性
  - 分类中可以写@property 但是不会生成getter和setter方法的实现，需要自己去写。
  - 分类中不可以直接访问本类的私有属性，但是可以调用sette和getter方法
1. ## 延展Extension
- 是一个特殊的分类
- 延展没有名字
- 延展只有声明，没有实现，和本类共享实现。
- 语法
  - 类名后面带个括号

```Objective-C
@interface LiveCore()
@end
// 没有实现
```

- 分类和延展的区别：
  
  - 分类有名字，延展没有名字是个匿名类 
  - 分类只能新增方法，延展都可以新增(方法，属性)
  - 分类写property，只会生成setter和getter的声明；延展中的property，会生成私有属性，并且生成setter和getter方法的声明和实现，只不过是在本类中生成。

- 什么时候使用延展
  
  - 要为类定义私有成员的时候，可以将延展定义在这个类的实现中。
1. ## _block
- block是种数据类型，专门用来存储某段代码  代码可以有参数和返回值

- ### 使用
  
  - block的声明

```Objective-C
 // 返回值类型 (^block变量的名称)(参数列表):
 void (^myBlock1)();    // myBlock1  无参数  无返回值
 int (^myBlock2)();
 int (^myBlock3)(int n1, int n2);
```

- 初始化block变量和执行

```Objective-C
// 返回值类型(参数列表){
//    代码段；
//}；
void (^myBlcok1)() = ^void() {
// codes
};
myBlcok1();
```

- 可以用typedef简化block

```Objective-C
typedef int (^NewType)(int n1, int n2);
NewType t1 = ^int(int n1, int n2)
{
// codes
}
```

- block内部访问外部变量
  - block内可以访问外部的局部变量和全局变量
  - 在block内部可以修改全局变量，但是不能修改局部变量
  - 如果希望修改外部的局部变量  需要用__block来修饰这个局部变量
- block可以作为参数  

```Objective-C
void test(void (^block1)()) {
    // 无返回值 无参数的block参数
}
```

- 作为函数返回值
1. ## 协议protocol(接口)
- ### 协议声明
  
  - 协议只有.h文件  只能声明方法

```Objective-C
@protocol 协议名称 <NSObject>
// 方法声明
@end
```

- 单继承 多协议

- 协议可以继承协议  并且协议间的继承可以是多继承

- ### 修饰关键字
  
  - @opational

当协议中方法被optional修饰的时候  当一个类继承该接口，就必须实现该协议

- @require(默认)

当协议中的方法被require修饰的时候 当一个类继承该接口 可选择是否实现

1. # Foundation相关

2. ## NSString
- 是专门用来存储OC字符串的类，必须使用@

- NSString 类的成语变量在@perproty最好用copy修饰 此时是浅拷贝

- ### NSString常用的方法

```Objective-C
// 将c字符串转换为NSString字符串
+stringWithUTF8String:(const char *)nullTerminatedCString;
// 拼接NSString字符串
+ (instancetype)localizedStringWithFormat:(NSString *)format;
// length方法 返回NSUInteger类型 得到字符串长度
@property (readonly) NSUInteger length;
// 得到字符串中的指定下标的字符  返回值是unichar
- (unichar)characterAtIndex:(NSUInteger)index;
// 判断两个字符串是否相等
- (BOOL)isEqualToString:(NSString *)aString;
// 比较字符串的大小 NSComparisonResult是个枚举类型  -1 0 1 
- (NSComparisonResult)compare:(NSString *)string; 
// 判断字符串是否以指定字符串开头
- (BOOL)hasPrefix:(NSString *)str;
// 判断字符串是否以指定字符串结尾
- (BOOL)hasSuffix:(NSString *)str;
// 搜索子串(从前往后)  返回的是NSRange的结构体
- (NSRange)rangeOfString:(NSString *)searchString;
typedef struct _NSRange{
    NSUInteger location; // 代表子串在主串中出现的下标  如果没有找到 为NSNotFound
    NSUInteger length; // 代表子串匹配的长度  如果没找到 为0
}NSRange; 

// 字符串的截取
// 1.从指定下标截取到最后
- (NSString *)substringFromIndex:(NSUInteger)from;
// 2.从开头截取指定个数
- (NSString *)substringToIndex:(NSUInteger)to;
// 3.截取某个范围
- (NSString *)substringWithRange:(NSRange)range;  

// 字符串替换
- (NSString *)stringByReplacingOccurrencesOfString:(NSString *)target withString:(NSString *)replacement options:(NSStringCompareOptions)options range:(NSRange)searchRange API_AVAILABLE(macos(10.5), ios(2.0), watchos(2.0), tvos(9.0));
```

### NSMutableString

- 用法

```Objective-C
// 1.新建
NSMutableString *ns = [NSMutableString string];

// 向NSMutableString中追加字符串
// - (void)appendString:(NSString *)aString;
// - (void)appendFormat:(NSString *)format, ... NS_FORMAT_FUNCTION(1,2);
[ns appendString : @"xxx"];
```

1. ## NSArray
- OC中的数组 只能存储oc对象 且长度固定  元素无法删除

- NAArray中存储的数组都是id类型的

- ### NSArray用法

```Objective-C
// 创建NSArray数组   数组长度为0
NSArray *arr1 = [NSArray new];
NSArray *arr2 = [[NSArray alloc] init];
NSArray *arr3 = [NSArray array];

// 创建数组并添加一个元素
+ (instancetype)arrayWithObject:(ObjectType)anObject;

// 创建数组 添加多个NS对象   并用nil进行结尾  表示数组结束
// + (instancetype)arrayWithObjects:(ObjectType)firstObj, ... NS_REQUIRES_NIL_TERMINATION;
 NSArray *arr3 = [NSArray arrayWithObjects:, nil]

 // 简化创建NS数组的方式
 NSArray arr = @[对象1,对象2，...];

 // 利用index取出指定下标的元素
 - (ObjectType)objectAtIndex:(NSUInteger)index;
 // 获取数组长度
 @property (readonly) NSUInteger count;
 // 是否包含某个元素
 - (BOOL)containsObject:(ObjectType)anObject;
 // 第一个元素(优于arr[0] 因为如果数组为空 arr[0]会报错)  和  最后一个元素
 // @property (nullable, nonatomic, readonly) ObjectType firstObject API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));
// @property (nullable, nonatomic, readonly) ObjectType lastObject;
arr1.firstObject;

// 指定元素第一次出现的下标  若没找到就返回NSUInteger的最大值
- (NSUInteger)indexOfObject:(ObjectType)anObject;

// NSArray遍历
// 1.for 
// 2.增强for for(object in arr)..
```

- ### NSMutableArray
  
  - NSMutableArray 元素可以动态改变
  - 用法

```Groovy
// 用法与NSArray一致
// 向NSMutableArray中添加NSArray对象
- (void)addObjectsFromArray:(NSArray<ObjectType> *)otherArray;

// 指定下标插入元素
- (void)insertObject:(ObjectType)anObject atIndex:(NSUInteger)index;

// 删除元素
- (void)removeObjectAtIndex:(NSUInteger)index;// 指定下标
- (void)removeObject:(ObjectType)anObject;// 指定元素
- (void)removeObject:(ObjectType)anObject inRange:(NSRange)range;// 指定范围
- (void)removeLastObject; // 最后一个元素
- (void)removeAllObjects;// all
```

1. ## NSNumber
- 用来包装基本数据类型
- NSNumber用法

```Objective-C
// 基本数据类型->NSNumber对象
NSNumber *n1 = [NSNumber numberWithInt : 12];
// NSNumber对象->基本数据类型
int val = n1.intValue;
// 转化成NSNumber可以用简写 @
NSNumber *n1 = @12;
int num = 10;
NSNumber *n2 = @(num);
```

1. ## NSDictionary
- Key-value  理解为map  实为字典数组

- NSDictionary 无法动态增删

- ### 用法

```Objective-C
// 1.NSDictionary创建
// + (instancetype)dictionaryWithObjectsAndKeys:(id)firstObject, ... NS_REQUIRES_NIL_TERMINATION NS_SWIFT_UNAVAILABLE("Use dictionary literals instead");
NSDictionary *dict = [NSDictionary dictionaryWithObjectsAndKeys : value,key,..., nil];
// 2.简单方式
NSDictionary *dict = @{key:value, key:value....};

// 2.NSDictionary取value值
dict[@"key"];

// 3.取NSDictionary中key-value的个数
@property (readonly) NSUInteger count;

// 4.遍历NSDictionary  可以用增强for循环
for (id item in dict) {
    // item会得到key  可以通过dict[item]得到value
}
```

- ### NSMutableDictionary
  
  - 在NSDictionary的基础上，增加了动态增删
  - 用法：

```Objective-C
// add object
// - (void)setObject:(ObjectType)anObject forKey:(KeyType <NSCopying>)aKey;
[dict setObject : @"value" forKey : @"key"];

// remove
- (void)removeAllObjects; // remove all
- (void)removeObjectsForKeys:(NSArray<KeyType> *)keyArray; // 删除某个key-value
```

1. ## NSFileManager
- 用来操作磁盘上的文件或者文件夹

- NSFileManager是以单例模式创建的

- ### 用法

```Objective-C
// 1.NSFileManager对象的创建 单例模式下
NSFileManager *fm = [NSFileManager defaultManager];

// 2.判断文件/文件夹是否存在
- (BOOL)fileExistsAtPath:(NSString *)path;
// 3.判断指定路径是否存在 并且判断是文件路径还是文件夹路径
// isDictionary == YES 文件夹路径k
- (BOOL)fileExistsAtPath:(NSString *)path isDirectory:(nullable BOOL *)isDirectory;
// 4.判断指定文件/文件夹是否可读
- (BOOL)isReadableFileAtPath:(NSString *)path;
// 5.判断指定文件/文件夹是否可写
- (BOOL)iWritableFileAtPath:(NSString *)path;
// 6.判断指定文件/文件夹是否可删除
- (BOOL)isDeletableFileAtPath:(NSString *)path;
// 7.获取指定目录下所有的子目录和文件
- (nullable NSArray<NSString *> *)subpathsAtPath:(NSString *)path;
// 8.在指定目录创建文件
- (BOOL)createFileAtPath:(NSString *)path contents:(nullable NSData *)data attributes:(nullable NSDictionary<NSFileAttributeKey, id> *)attr;
// 9.拷贝文件
- (BOOL)copyItemAtPath:(NSString *)srcPath toPath:(NSString *)dstPath error:(NSError **)
// 10.移动文件 剪切 文件重命名
- (BOOL)moveItemAtPath:(NSString *)srcPath toPath:(NSString *)dstPath error:(NSError **)error
// 11.删除文件
- (BOOL)removeItemAtPath:(NSString *)path error:(NSError **)error
```

1. ## NSDate
- 时间处理类 
- 用法

```Objective-C
// 1.创建
NSDate *da = [NSDate date];
```

1. ## copy
- 定义在NSObject中，用来拷贝对象

- copy方法内部调用了另一个方法 copyWithZone，copyWithZone定义在NSCoping协议中。

- ### copy应用在NSString和NSMutableString
  
  - NSString
    - copy在NSString中发生的是浅拷贝
  - NSMutableString
    - copy在NSMutable中发生的是深拷贝，但是拷贝出来的对象是NSString类型的，不可变字符串。

- ### mutableCopy
  
  - NSString在进行mutableCopy的时候发生的是深拷贝，产生的可变字符串
  - NSMutableString在mutableCopy的时候发生的是深拷贝，产生的是可变字符串

- ### 自定义对象的copy

```Objective-C
Person *pp = [Person new];
[pp copy];
```

- 如果希望自定义的类具有对象拷贝能力，就要遵守NSCoping协议并实现copyWithZone方法

```Objective-C
-(id)copyWithZone : (NSZone*)zone
{
    // codes
    return self; // 浅拷贝
}
```

- 根据实际情况做深浅拷贝