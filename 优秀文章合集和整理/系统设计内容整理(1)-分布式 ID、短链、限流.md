
# 系统设计内容整理(1)-分布式 ID、短链、限流

  

> 关键词：分布式 ID  短链  限流


# 分布式 ID 相关

  

> [vivo 自研鲁班分布式 ID 服务实践](https://mp.weixin.qq.com/s/MHhxU_G0d303R-b6T6EcoQ)


> [分布式唯一 ID 浅谈](https://mp.weixin.qq.com/s/10hn22MInanJXuT6wOYh1Q)


> [分布式 ID 生成服务的技术原理和项目实战](https://mp.weixin.qq.com/s/bFDLb6U6EgI-DvCdLTq_QA)

  ![](https://fastly.jsdelivr.net/gh/leexsh/picture_save/202310162322429.png)

在业务开发中，大量场景需要唯一 ID 来进行标识：用户独一无二的身份认证、超市售卖的商品、微信的即时消息，它们都需要标识来确定唯一性。需要在特定范围内保证 ID 具备唯一性，这是 ID 生成服务最基本的要求。

## 特性


① 唯一性：生成的 ID 唯一，特定范围不冲突；

② 有序性：生成的 ID 按某种规则有序，趋势递增，便于入库和查询，但不严格要求；

③ 高可用、高性能：高并发下的具备高可用，确保任何情况能容灾，稳定提供服务；

④ 自主性：分布式环境下不依赖中心认证即可自行生成 ID；

⑤ 安全性：脱敏，不暴露系统和业务的信息，如：订单数，用户数。

## 分布式 ID 方案

### UUID

- UUID 的标准形式为 32 个十六进制数组成的字符串，且分割为五个部分，例如：467e8542-2275-4163-95d6-7adc205580a9;

- 可以基于时间、随机数等生成，可以完全在本地生成，不依赖网络；但是会造成 MAC 地址泄露等问题。

### 数据库自增 ID

- 数据库自增 ID 是最常见的一种生成 ID 方式。利用数据库本身来进行设置，在全数据库内保持唯一。优势是使用简单，满足基本业务需求，天然有序；缺点是强依赖 DB，会由于数据库部署的一些特性而存在单点故障、数据一致性等问题。

- 针对上面介绍的数据库自增 ID 的缺陷，会存在以下两种优化方案：

  1. 数据库水平拆分，设置不同的初始值和相同的步长。这样可以有效的生成集群中的唯一 ID，也大大降低 ID 生成数据库操作的负载。

  2. 批量生成一批 ID。这样可以将数据库的压力减小到先前的 N 分之一，且数据库故障后仍可继续使用一段时间。

- 数据库自增 ID 方案的优势是非常简单，可利用现有数据库系统的功能实现；ID 号单调自增。其缺陷包括强依赖 DB，当 DB 异常时整个系统将处于不可用的状态；ID 号的生成速率取决于所使用数据库的读写性能。

### Redis 生成 ID

- 可以尝试使用 Redis 来生成 ID，主要使用 Redis 的原子操作 INCR 和 INCRBY 来实现。优势是不依赖于数据库，使用灵活，性能也优于数据库；而缺点则是可能要引入新的组件 Redis，如果 Redis 出现单点故障问题，则会影响序号服务的可用性。

### Zookeeper 生成 ID

- 主要是利用 Zookeeper 的 znode 数据版本来生成序列号，可以生成 32 位和 64 位的数据版本号，客户端可以使用这个版本号来作为唯一的序列号。由于需要依赖 zookeeper，并且是多步调用 API，如果在竞争较大的情况下，可能需要考虑使用分布式锁，故此种生成唯一 ID 的方法的性能在高并发的分布式环境下不甚理想

### 雪花算法

- snowflake(雪花算法)是一个开源的分布式 ID 生成算法，结果是一个 long 型的 ID。snowflake 算法将 64bit 划分为多段，分开来标识机器、时间等信息，具体组成结构如下图所示：

![](https://fastly.jsdelivr.net/gh/leexsh/picture_save/202310292243459.png)

snowflake 算法的核心思想是使用 41bit 作为毫秒数，10bit 作为机器的 ID（比如其中 5 个 bit 可作为数据中心，5 个 bit 作为机器 ID）,12bit 作为毫秒内的流水号（意味着每个节点在每毫秒可以产生 4096 个 ID），最后还有一个符号位，永远是 0。
# 短链设计


> [短链系统设计](https://mp.weixin.qq.com/s/lmI98_UBTUwpfMkNWiu2Nw)


> [转转短链平台设计与实现 - 掘金](https://juejin.cn/post/7265155568398434343)


通过短链系统可以把长链进行映射，方便传播。点击短链后，会通过重定向(301、302)的方式，映射到长链。


![](https://fastly.jsdelivr.net/gh/leexsh/picture_save/202310292243455.png)

## 短链的生成方式

短链通常通过哈希或者分布式 ID 的方式进行生成。
### 哈希

常见的哈希算法就是 MD5、SHA 等；能够满足这样要求的简单的哈希算法有很多，其中比较著名并且应用广泛的一个哈希算法，那就是 <strong>MurmurHash </strong>算法。尽管这个哈希算法在 2008 年才被发明出来，但现在它已经广泛应用到 Redis、MemCache、Cassandra、HBase、Lucene 等众多著名的软件中。

MurmurHash 算法提供了两种长度的哈希值，一种是 32bits，一种是 128bits。为了让最终生成的短链尽可能短，我们可以选择 32bits 的哈希值。比如假设某个长链接经过 MurmurHash 计算后得到的哈希值是 181343514，再拼上短链服务的域名就变成了最终的短链 [http://xxx.cn/181343514](http://xxx.cn/181343514)。

但是 MurmurHash 算法得到的短链还是很长，可以将 10 进制的哈希值，转化成更高进制的哈希值，这样哈希值就变短了。在网址 URL 中，常用的合法字符有 0～9、a～z、A～Z 这样 62 个字符。为了让哈希值表示起来尽可能短，我们可以将 10 进制的哈希值转化成 62 进制，最终用 62 进制表示的短链就是 [http://xxx.cn/cgTJo](http://xxx.cn/cgTJo).

1. <strong>解决哈希冲突:</strong>

哈希算法无法避免的一个问题，就是哈希冲突。一旦冲突，就会导致两个原始网址被转化成同一个短链。当用户访问短链的时候，我们就无从判断，用户想要访问的是哪一个原始网址了。

一般情况下，我们会保存短链跟原始网址之间的对应关系，以便后续用户在访问短链的时候，可以根据对应关系，查找到原始网址。可以设计存储系统或者利用现成的数据库比如 MySQL、Redis 去避免这个问题。

以 MySQL 为例，当有一个新的原始网址需要生成短链的时候，我们生成短链。然后将这个新生成的短链，在 MySQL 数据库中查找：

- 如果没有找到相同的短链，这就表明这个新生成的短链没有冲突。于是我们就将这个短链返回给用户，然后将这个短链与原始网址之间的对应关系，存储到 MySQL 数据库中

- 如果在数据库中找到了<strong>相同的短链</strong>，那也并不一定说明就冲突了。我们先从数据库中将这个短链对应的原始网址取出来：

  - 如果数据库中的原始网址，跟我们现在正在处理的原始网址是一样的，这就说明已经有人请求过这个原始网址的短链了。我们就可以拿这个短链直接用。

  - 如果数据库中记录的原始网址，跟我们正在处理的原始网址不一样，那就说明哈希算法发生了冲突。不同的原始网址，经过计算，得到的短链重复了。可以给原始网址拼接一串特殊字符，比如 `DUPLICATED`，然后再重新计算哈希值，两次哈希计算都冲突的概率会是非常低的。假设出现非常极端的情况，又发生冲突了，我们可以再换一个拼接字符串，比如 `XXXXX`，再计算哈希值。把计算得到的哈希值，跟原始网址拼接了特殊字符串之后的文本，一并存储在 MySQL 数据库中。

  - 当用户访问短链的时候，短链服务先通过短链，在数据库中查找到对应的原始网址。如果原始网址有拼接特殊字符（这个很容易通过字符串匹配算法找到），就先将特殊字符去掉，然后再将不包含特殊字符的原始网址返回给浏览器。

1. <strong>性能</strong>

在短链生成的过程中，服务器会执行两条 SQL 语句：

第二步是无法避免的，而第一步可以通过给短链字段建立<strong>唯一索引</strong>来优化，当有新的原始网址需要生成短链的时候，并不会拿生成的短链在数据库中查找判重，而是直接将生成的短链与对应的原始网址尝试存储到数据库中。如果数据库能够将数据正常写入，那说明并没有违反唯一索引，也就是说，这个新生成的短链并没有冲突。

如果数据量非常大，冲突概率大幅上升，这种情况下该怎么办？可以使用<strong>布隆过滤器</strong>。把已经生成的短链，构建成布隆过滤器。当有新的短链生成的时候，我们先拿这个新生成的短链，在布隆过滤器中查找。如果查找的结果是不存在，那就说明这个新生成的短链并没有冲突。这个时候，我们只需要再执行写入短链和对应原始网页的 SQL 语句就可以了。

### 分布式 ID

可以使用分布式 ID 生成唯一的短链地址，参考分布式 ID 内容。生成 ID 后，转化成 62 进制表示法，拼接到短链服务的域名（比如 [http://xxxx.cn/](http://xxxx.cn/)）形成了短链。最后,还是会把生成的短链和对应的原始网址存储到数据库中。

1. <strong>存在的问题--相同的原始网址可能会对应不同的短链</strong>

每次新来一个原始网址，我们就生成一个新的短链，这种做法就会导致两个相同的原始网址生成了不同的短链。这个该如何处理呢？实际上，我们有两种处理思路。

# 限流

> GO:[常用限流算法的应用场景和实现原理](https://zhuanlan.zhihu.com/p/338368004)


> Java:[从计数器到令牌桶 | 限流方法大揭秘 - 掘金](https://juejin.cn/post/7270486384104144956)


限流可以认为服务降级的一种，限流就是限制系统的输入和输出流量已达到保护系统的目的。为了保证系统的稳定运行，一旦达到的需要限制的阈值，就需要限制流量并采取一些措施以完成限制流量的目的。比如：延迟处理，拒绝处理，或者部分拒绝处理等等。

## 限流的几种方式

### 计数器

计数器是一种比较简单粗暴的限流算法，其思想是在固定时间窗口内对请求进行计数，与阀值进行比较判断是否需要限流，一旦到了时间临界点，将计数器清零。

1. <strong>计数器的劣势</strong>

计数器算法存在“时间临界点”缺陷。比如每一分钟限制 100 个请求，可以在 00:00:00-00:00:58 秒里面都没有请求，在 00:00:59 瞬间发送 100 个请求，这个对于计数器算法来是允许的，然后在 00:01:00 再次发送 100 个请求，意味着在短短 1s 内发送了 200 个请求，如果量更大呢，系统可能会承受不住瞬间流量，导致系统崩溃。

1. <strong>代码实现</strong>

```go

type LimitRate struct {

   rate  int           //阀值

   begin time.Time     //计数开始时间

   cycle time.Duration //计数周期

   count int           //收到的请求数

   lock  sync.Mutex    //锁

}

  

func (limit *LimitRate) Allow() bool {

   limit.lock.Lock()

   defer limit.lock.Unlock()

  

   // 判断收到请求数是否达到阀值

   if limit.count == limit.rate-1 {

      now := time.Now()

      // 达到阀值后，判断是否是请求周期内

      if now.Sub(limit.begin) >= limit.cycle {

         limit.Reset(now)

         return true

      }

      return false

   } else {

      limit.count++

      return true

   }

}

  

func (limit *LimitRate) Set(rate int, cycle time.Duration) {

   limit.rate = rate

   limit.begin = time.Now()

   limit.cycle = cycle

   limit.count = 0

}

  

func (limit *LimitRate) Reset(begin time.Time) {

   limit.begin = begin

   limit.count = 0

}

```

### 滑动窗口


滑动窗口算法将一个大的时间窗口分成多个小窗口，每次大窗口向后滑动一个小窗口，并保证大的窗口内流量不会超出最大值，这种实现比固定窗口的流量曲线更加平滑。

普通时间窗口有一个问题，比如窗口期内请求的上限是 100，假设有 100 个请求集中在前 1s 的后 100ms，100 个请求集中在后 1s 的前 100ms，其实在这 200ms 内就已经请求超限了，但是由于时间窗每经过 1s 就会重置计数，就无法识别到这种请求超限。

对于滑动时间窗口，我们可以把 1ms 的时间窗口划分成 10 个小窗口，或者想象窗口有 10 个时间插槽 time slot, 每个 time slot 统计某个 100ms 的请求数量。每经过 100ms，有一个新的 time slot 加入窗口，早于当前时间 1s 的 time slot 出窗口。窗口内最多维护 10 个 time slot。

1. <strong>代码实现</strong>

```go

type timeSlot struct {
    timestamp time.Time // 这个timeSlot的时间起点
    count     int       // 落在这个timeSlot内的请求数
}

// 统计整个时间窗口中已经发生的请求次数

func countReq(win []*timeSlot) int {
    var count int
    for _, ts := range win {
        count += ts.count
    }
    return count
}

type SlidingWindowLimiter struct {
    mu           sync.Mutex    // 互斥锁保护其他字段
    SlotDuration time.Duration // time slot的长度
    WinDuration  time.Duration // sliding window的长度
    numSlots     int           // window内最多有多少个slot
    windows      []*timeSlot
    maxReq       int // 大窗口时间内允许的最大请求数
}

func NewSliding(slotDuration time.Duration, winDuration time.Duration, maxReq int) *SlidingWindowLimiter {
    return &SlidingWindowLimiter{
        SlotDuration: slotDuration,
        WinDuration:  winDuration,
        numSlots:     int(winDuration / slotDuration),
        maxReq:       maxReq,
    }
}


func (l *SlidingWindowLimiter) validate() bool {
    l.mu.Lock()
    defer l.mu.Unlock()
    now := time.Now()
    // 已经过期的time slot移出时间窗
    timeoutOffset := -1
    for i, ts := range l.windows {
        if ts.timestamp.Add(l.WinDuration).After(now) {
            break
        }
        timeoutOffset = i
    }
    if timeoutOffset > -1 {
        l.windows = l.windows[timeoutOffset+1:]
    }

    // 判断请求是否超限
    var result bool
    if countReq(l.windows) < l.maxReq {
        result = true
    }

    // 记录这次的请求数

    var lastSlot *timeSlot
    if len(l.windows) > 0 {
        lastSlot = l.windows[len(l.windows)-1]
        if lastSlot.timestamp.Add(l.SlotDuration).Before(now) {
            // 如果当前时间已经超过这个时间插槽的跨度，那么新建一个时间插槽

            lastSlot = &timeSlot{timestamp: now, count: 1}
            l.windows = append(l.windows, lastSlot)
        } else {
            lastSlot.count++
        }

    } else {

        lastSlot = &timeSlot{timestamp: now, count: 1}

        l.windows = append(l.windows, lastSlot)

    }
    return result

}

```

### 漏桶

漏桶算法是首先想象有一个木桶，桶的容量是固定的。当有请求到来时先放到木桶中，处理请求的 `worker` 以固定的速度从木桶中取出请求进行相应。如果木桶已经满了，直接返回请求频率超限的错误码或者页面。

![](https://fastly.jsdelivr.net/gh/leexsh/picture_save/202310292243460.png)
1. <strong>劣势</strong>

木桶流入请求的速率是不固定的，但是流出的速率是恒定的。这样的话能保护系统资源不被打满，但是面对<strong>突发流量时会有大量请求失败</strong>，不适合电商抢购和微博出现热点事件等场景的限流。
1. <strong>代码实现</strong>

```go
// 漏桶
// 一个固定大小的桶，请求按照固定的速率流出
// 如果桶是空的，不需要流出请求
// 请求数大于桶的容量，则抛弃多余请求

type LeakyBucket struct {
   rate       float64    // 每秒固定流出速率
   capacity   float64    // 桶的容量
   water      float64    // 当前桶中请求量
   lastLeakMs int64      // 桶上次漏水微秒数
   lock       sync.Mutex // 锁
}

func (leaky *LeakyBucket) Allow() bool {
   leaky.lock.Lock()
   defer leaky.lock.Unlock()
   now := time.Now().UnixNano() / 1e6
   // 计算剩余水量,两次执行时间中需要漏掉的水
   leakyWater := leaky.water - (float64(now-leaky.lastLeakMs) * leaky.rate / 1000)
   leaky.water = math.Max(0, leakyWater)
   leaky.lastLeakMs = now
   if leaky.water+1 <= leaky.capacity {
      leaky.water++
      return true
   } else {
      return false
   }
}

func (leaky *LeakyBucket) Set(rate, capacity float64) {
   leaky.rate = rate
   leaky.capacity = capacity
   leaky.water = 0
   leaky.lastLeakMs = time.Now().UnixNano() / 1e6
}

```

### 令牌桶
令牌桶是反向的"漏桶"，它是以恒定的速度往木桶里加入令牌，木桶满了则不再加入令牌。服务收到请求时尝试从木桶中取出一个令牌，如果能够得到令牌则继续执行后续的业务逻辑。如果没有得到令牌，直接返回访问频率超限的错误码或页面等，不继续执行后续的业务逻辑。

![](static/LoTnbLEgloqcifx0rS9cqNNcnJg.png)
同时由于往木桶添加令牌的速度是恒定的，且木桶的容量有上限，所以单位时间内处理的请求书也能够得到控制，起到限流的目的。假设加入令牌的速度为 1token/10ms，桶的容量为 500，在请求比较的少的时候（小于每 10 毫秒 1 个请求）时，木桶可以先"攒"一些令牌（最多 500 个）。当有突发流量时，一下把木桶内的令牌取空，也就是有 500 个在并发执行的业务逻辑，之后要等每 10ms 补充一个新的令牌才能接收一个新的请求。

适合电商抢购或者微博出现热点事件这种场景，因为在限流的同时可以应对一定的突发流量。如果采用漏桶那样的均匀速度处理请求的算法，在发生热点时间的时候，会造成大量的用户无法访问，对用户体验的损害比较大。

```go
type TokenBucket struct {
   rate         int64 //固定的token放入速率, r/s
   capacity     int64 //桶的容量
   tokens       int64 //桶中当前token数量
   lastTokenSec int64 //上次向桶中放令牌的时间的时间戳，单位为秒
   lock sync.Mutex
}

func (bucket *TokenBucket) Take() bool {
   bucket.lock.Lock()
   defer bucket.lock.Unlock()
   now := time.Now().Unix()
   bucket.tokens = bucket.tokens + (now-bucket.lastTokenSec)*bucket.rate // 先添加令牌
   if bucket.tokens > bucket.capacity {
      bucket.tokens = bucket.capacity
   }

   bucket.lastTokenSec = now
   if bucket.tokens > 0 {
      // 还有令牌，领取令牌
      bucket.tokens--
      return true
   } else {
      // 没有令牌,则拒绝
      return false
   }
}

func (bucket *TokenBucket) Init(rate, cap int64) {
   bucket.rate = rate
   bucket.capacity = cap
   bucket.tokens = 0
   bucket.lastTokenSec = time.Now().Unix()

}

```