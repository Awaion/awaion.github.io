# Reactor 

# 主要内容

> [如何提升程序性能](#如何提升程序性能)  
> [多线程](#多线程)  
> [异步](#异步)  
> [响应式编程](#响应式编程)  
> [Reactor](#reactor)  
> [Reactor 核心特性](#reactor-核心特性)  


## 如何提升程序性能

现代应用需要应对大量的并发用户,而且即使现代硬件的处理能力飞速发展,软件性能仍然是关键因素.
广义来说我们有两种思路来提升程序性能:
1. 并行化(parallelize)：使用更多的线程和硬件资源
2. 基于现有的资源来提高执行效率

## 多线程

通常Java开发者使用阻塞式(blocking)编写代码.在出现性能瓶颈后,我们可以增加处理线程,线程中同样是阻塞的代码.但是这种使用资源的方式会迅速面临资源竞争
和并发问题.

更糟糕的是,阻塞会浪费资源.具体来说,比如当一个程序面临延迟(通常是I/O方面,比如数据库读写请求或网络调用),所在线程需要进入 idle 状态等待数据,从而浪
费资源.所以并行化方式并非银弹.这是挖掘硬件潜力的方式,但是却带来了复杂性,而且容易造成浪费.

## 异步

第二种思路,提高执行效率可以解决资源浪费问题.通过编写异步非阻塞的代码,任务发起异步调用后,执行过程会切换到另一个使用同样底层资源的活跃任务,然后等异步
调用返回结果再去处理.

Java 提供了两种异步编程方式:
1. 回调(Callbacks): 异步方法没有返回值,而是采用一个 callback 作为参数(lambda 或匿名类),当结果出来后回调这个 callback.常见的例子比如 
Swings 的 EventListener.回调很难组合起来,因为很快就会导致代码难以理解和维护,即回调地狱(callback hell)
2. Futures: 异步方法立即返回一个 Future<T>,该异步方法要返回结果的是 T 类型,通过 Future封装.这个结果并不是立刻可以拿到,而是等实际处理结束才
可用.比如 ExecutorService 执行 Callable<T> 任务时会返回 Future 对象.Futures 比回调要好一点,但即使在 Java 8 引入了 CompletableFuture, 
它对于多个处理的组合仍不够好用.当对 Future 对象最终调用 get() 方法时,仍然会导致阻塞,并且缺乏对多个值以及更进一步对错误的处理.

## 响应式编程

响应式编程是一种关注于数据流(data streams)和变化传递(propagation of change)的异步编程方式.它可以用既有的编程语言表达静态(如数组)或动态
(如事件源)的数据流.

在响应式编程方面,微软跨出了第一步,它在 .NET 生态中创建了响应式扩展库(Reactive Extensions library, Rx).接着 RxJava 在 JVM 上实现了响应式
编程.后来,在 JVM 平台出现了一套标准的响应式编程规范,它定义了一系列标准接口和交互规范.并整合到 Java 9 中(使用 Flow 类)

响应式编程通常作为面向对象编程中的观察者模式(Observer design pattern)的一种扩展.响应式流(reactive streams)与迭代子模式
(Iterator design pattern)也有相通之处,因为其中也有 Iterable-Iterator 这样的对应关系.主要的区别在于,Iterator 是基于拉取(pull)方式的,而
响应式流是基于推送(push)方式的.

使用 iterator 是一种命令式(imperative)编程范式,即使访问元素的方法是 Iterable 的唯一职责.关键在于,什么时候执行 next() 获取元素取决于开发者.
在响应式流中,相对应的 角色是 Publisher-Subscriber,但是当有新的值到来的时候,却反过来由发布者(Publisher)通知订阅者(Subscriber),这种推送模式
是响应式的关键.此外,对推送来的数据的操作是通过一种声明式(declaratively)而不是命令式(imperatively)的方式表达的:开发者通过描述控制流程来定义对
数据流的处理逻辑.

除了数据推送,对错误处理(error handling)和完成(completion)信号的定义也很完善.一个 Publisher 可以推送新的值到它的 Subscriber
(调用 onNext 方法),同样也可以推送错误(调用 onError 方法)和完成(调用 onComplete 方法)信号.错误和完成信号都可以终止响应式流,可以表达式描述:
`onNext x 0..N [onError | onComplete]`

类似 Reactor 这样的响应式库的目标就是要弥补上述经典的 JVM 异步方式所带来的不足,此外还会关注一下几个方面:
- 可编排性(Composability)
- 可读性(Readability)
- 使用丰富的操作符来处理形如流的数据
- 在订阅(subscribe)之前什么都不会发生
- 背压(backpressure)具体来说即消费者能够反向告知生产者生产内容的速度的能力
- 高层次同时也是有高价值的的抽象,从而达到并发无关的效果

1. 可编排性
指的是编排多个异步任务的能力.比如我们将前一个任务的结果传递给后一个任务作为输入,或者将多个任务以分解再汇总(fork-join)的形式执行,或者将异
步的任务作为离散的组件在系统中进行重用.这种编排任务的能力与代码的可读性和可维护性是紧密相关的.随着异步处理任务数量和复杂度的提高,编写和阅读代码都变
得越来越困难.就像我们刚才看到的,回调模式是简单的,但是缺点是在复杂的处理逻辑中,回调中会层层嵌入回调,导致回调地狱(Callback Hell)这样的代码是难以阅
读和分析的. Reactor 提供了丰富的编排操作,从而代码直观反映了处理流程,并且所有的操作保持在同一层次(尽量避免了嵌套)

2. 可读性
你可以想象数据在响应式应用中的处理,就像流过一条装配流水线.Reactor 既是传送带,又是一个个的装配工或机器人.原材料从源头(最初的 Publisher)流出,最终
被加工为成品,等待被推送到消费者或者说 Subscriber. 原材料会经过不同的中间处理过程,或者作为半成品与其他半成品进行组装.如果某处有齿轮卡住,或者某件
产品的包装过程花费了太久时间,相应的工位就可以向上游发出信号来限制或停止发出原材料.

3. 操作符
在 Reactor 中,操作符(operator)就像装配线中的工位操作员或装配机器人.每一个操作符对 Publisher 进行相应的处理,然后将 Publisher 包装为一个新的
Publisher,就像一个链条,数据源自第一个 Publisher,然后顺链条而下,在每个环节进行相应的处理.最终,一个订阅者(Subscriber)终结这个过程.订阅者不订
阅,则什么都不会发生.理解了操作符会创建新的 Publisher 实例这一点,能够帮助你避免一个常见的问题,这种问题会让你觉得处理链上的某个操作符没有起作用.
虽然响应式流规范(Reactive Streams specification)没有规定任何操作符,类似 Reactor 这样的响应式库所带来的最大附加价值之一就是提供丰富的操作符.
包括基础的转换操作,到过滤操作,甚至复杂的编排和错误处理操作.

4. subscribe() 之前什么都不会发生
在 Reactor 中,当你创建了一条 Publisher 处理链,数据还不会开始生成.事实上,你是创建了一种抽象的对于异步处理流程的描述从而方便重用和组装,当真正订阅
subscribe() 的时候,你需要将 Publisher 关联到一个 Subscriber 上,然后才会触发整个链的流动.这时候 Subscriber 会向上游发送一个 request 信号,
一直到达源头 的 Publisher.

5. 背压
向上游传递信号这一点也被用于实现背压,就像在装配线上,某个工位的处理速度如果慢于流水线速度,会对上游发送反馈信号一样. 在响应式流规范中实际定义的机制同
刚才的类比非常接近,订阅者可以无限接受数据并让它的源头满负荷推送所有的数据,也可以通过使用 request 机制来告知源头它一次最多能够处理 n 个元素.中间环
节的操作也可以影响request.想象一个能够将每10个元素分批打包的缓存(buffer)操作.如果订阅者请求一个元素,那么对于源头来说可以生成10个元素.此外预取策
略也可以使用了,比如在订阅前预先生成元素,这样能够将'推送'模式转换为'推送+拉取'混合的模式,如果下游准备好了,可以从上游拉取n个元素,但是如果上游元素还
没有准备好,下游还是要等待上游的推送.

6. 热冷
在 Rx 家族的响应式库中,响应式流分为'热'和'冷'两种类型,区别主要在于响应式流如何对订阅者进行响应:

一个'冷'的序列,指对于每一个 Subscriber,都会收到从头开始所有的数据.如果源头生成了一个 HTTP 请求,对于每一个订阅都会创建一个新的 HTTP 请求.

一个'热'的序列,指对于一个 Subscriber,只能获取从它开始订阅之后发出的数据,不过注意,有些'热'的响应式流可以缓存部分或全部历史数据.通常意义上来说.
一个'热'的响应式流,甚至在即使没有订阅者接收数据的情况下,也可以发出数据(这一点同 Subscribe() 之前什么都不会发生的规则有冲突).

## Reactor

Reactor 是一个用于JVM的完全非阻塞的响应式编程框架,具备高效的需求管理(即对背压(backpressure)的控制)能力,它与 Java 8 函数式 API 直接集成,比
如 CompletableFuture, Stream 以及 Duration.它提供了异步序列 API Flux(用于[N]个元素)和 Mono(用于 [0|1]个元素),并完全遵循和实现了
响应式扩展规范(Reactive Extensions Specification)

Reactor 的 reactor-ipc 组件还支持非阻塞的进程间通信(inter-process communication, IPC). Reactor IPC 为 HTTP(包括 Websockets),TCP
和 UDP 提供了支持背压的网络引擎,从而适合应用于微服务架构.并且完整支持响应式编解码(reactive encoding and decoding).

Reactor 使用的技术
- Java's functional API
- CompletableFuture
- Stream
- Duration
- Flux [N]
- Mono [0|1]
- Reactive
- NON-BLOCKING IO

https://projectreactor.io/

https://projectreactor.io/docs/core/release/reference/

## Reactor 核心特性

- Mono: 0|1 数据流, Flux: N数据流
- subscribe() 订阅,自定义流的信号感知回调
- cancle() 消费者调用取消流的订阅
- BaseSubscriber 自定义消费者推荐直接编写 BaseSubscriber 的逻辑
- buffer() 缓冲
- limitRate() 限流
- handle() 自定义流中元素处理规则
- publishOn() 自定义线程调度
- onErrorReturn() 错误处理
- onErrorResume() 吃掉异常,兜底方法

```text
public class ReactorFluxDemo {

    public static void main(String[] args) throws Exception {
        // 数据扁平化(拆分)
//        flatMap();

        // 转换
//        transform();

        // 空数据设置默认值
//        switchIfEmpty();

        // 合并
//        merge();

        // 结对
//        zipWith();

        // 重试
//        retry();

        // 接收器,管道
//        sinks();

        // 缓存
//        cache();

        // 多线程
//        parallel();

        // 上下文
//        context();

        // 阻塞
//        block();

        // 忽略异常并记录,继续执行
//        onErrorContinue();

        // 最终操作
//        doFinally();

        // 捕获异常,记录特殊的错误日志,重新抛出
//        doOnError2();

        // 包装重新抛出异常;吃掉异常,消费者有感知;抛新异常;流异常完成
//        onErrorMap();

        // 吃掉异常,消费者有感知;调用一个自定义方法;流异常完成
//        onErrorResume2();

        // 吃掉异常,消费者无异常感知;调用一个兜底方法;流正常完成
//        onErrorResume();

        // 吃掉异常,消费者无异常感知;返回一个兜底默认值;流正常完成
//        onErrorReturn();

        // 自定义线程调度
//        scheduler();

        // 自定义流中元素处理规则
//        handle();

        // 限流
//        limitRate();

        // 数据缓冲量
//        buffer();

        // 自定义消费者
//        baseSubscribe();

        // 自定义流的信号感知回调
//        subscribe();

        // flux使用
//        flux();

        // subscribe 事件回调,钩子函数(Hook),doOnXxx
//        fluxSubscribeDoOn();

        // doOn 钩子(Hook)函数 流中某个元素到达以后触发我一个回调,卸载触发流的后面,新流的前面
//        doOnXxxx();

        // 连接合并
//        concat();

        // 执行顺序
//        executionOrder();


        //今天： Flux、Mono、弹珠图、事件感知API、每个操作都是操作的上个流的东西
    }

    private static void flatMap() {
        Flux.just("zhang san", "li si")
                .flatMap(v -> {
                    String[] s = v.split(" ");
                    return Flux.fromArray(s); // 把数据包装成多元素流
                })
                .log()
                .subscribe(); // 两个人的名字按照空格拆分,打印出所有的姓与名
    }

    private static void transform() {
        AtomicInteger atomic = new AtomicInteger(0);
        Flux<String> flux = Flux.just("a", "b", "c")
                .transform(values -> {
                    if (atomic.incrementAndGet() == 1) {
                        return values.map(String::toUpperCase);
                    } else {
                        return values;
                    }
                });

        //transform 无defer,不会共享外部变量的值.无状态转换原理,无论多少个订阅者,transform只执行一次
        //transform 有defer,会共享外部变量的值.有状态转换;原理,无论多少个订阅者,每个订阅者transform都只执行一次
        flux.subscribe(v -> System.out.println("订阅者1：v = " + v));
        flux.subscribe(v -> System.out.println("订阅者2：v = " + v));
    }

    private static void switchIfEmpty() {
        Mono.empty()
                .switchIfEmpty(Mono.just("兜底数据..."))
                .subscribe(v -> System.out.println("v = " + v));
    }

    private static void merge() throws IOException {
        // concat 连接 A流 所有元素和 B流所有元素拼接
        // merge 合并 A流 所有元素和 B流所有元素 按照时间序列合并
        // mergeWith
        // mergeSequential 按照哪个流先发元素排队
//        Flux.mergeSequential();
        Flux.merge(
                        Flux.just(1, 2, 3).delayElements(Duration.ofSeconds(1)),
                        Flux.just("a", "b").delayElements(Duration.ofMillis(1500)),
                        Flux.just("q", "w", "e", "r").delayElements(Duration.ofMillis(500)))
                .log()
                .subscribe();

//        Flux.just(1, 2, 3).mergeWith(Flux.just(4, 5, 6)).log().subscribe();
        System.in.read();
    }

    private static void zipWith() {
        Flux.just(1, 2, 3)
                .zipWith(Flux.just("a", "b", "c", "d"))
                .map(tuple -> {
                    Integer t1 = tuple.getT1(); // 元组中的第一个元素
                    String t2 = tuple.getT2(); // 元组中的第二个元素
                    return t1 + "==>" + t2;
                })
                .log()
                .subscribe(v -> System.out.println("v = " + v));
    }

    private static void retry() throws IOException {
        Flux.just(1)
                .delayElements(Duration.ofSeconds(3))
                .log()
                .timeout(Duration.ofSeconds(2))
                .retry(2) // 把流从头到尾重新请求一次
                .onErrorReturn(2)
                .map(i -> i + ":map")
                .subscribe(v -> System.out.println("v = " + v));
        System.in.read();
    }

    private static void cache() throws IOException {
        Flux<Integer> cache = Flux.range(1, 10)
                .delayElements(Duration.ofSeconds(1))
                .cache(2);   // 缓存两个元素,默认全部缓存
        cache.subscribe();

        // 自定义订阅者
        new Thread(() -> {
            try {
                Thread.sleep(5000);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
            cache.subscribe(v -> System.out.println("v = " + v));
        }).start();

        System.in.read();
    }

    private static void sinks() {
        //        Flux.create(fluxSink -> fluxSink.next("111"));

        // Sinks 接收器,数据管道,所有数据顺着这个管道往下走的

        // 发送Flux数据
//        Sinks.many();
        // 发送Mono数据
//        Sinks.one();

        // 单播 这个管道只能绑定单个订阅者(消费者)
//        Sinks.many().unicast();
        // 多播 这个管道能绑定多个订阅者
//        Sinks.many().multicast();
        // 重放 这个管道能重放元素,是否给后来的订阅者把之前的元素依然发给它
//        Sinks.many().replay();

        // 从头消费还是从订阅的那一刻消费
//        Sinks.Many<Object> many = Sinks.many()
//                .multicast()
//                .onBackpressureBuffer(); // 背压队列

        // 发布者数据重放,底层利用队列进行缓存之前数据 默认订阅者从订阅的那一刻开始接元素
        Sinks.Many<Object> many = Sinks.many().replay().limit(3);
        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                many.tryEmitNext("a-" + i);
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }).start();
        // 订阅
        many.asFlux().subscribe(v -> System.out.println("v1 = " + v));
        new Thread(() -> {
            try {
                Thread.sleep(5000);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
            many.asFlux().subscribe(v -> System.out.println("v2 = " + v));
        }).start();
    }

    private static void parallel() throws IOException {
        Flux.range(1, 1000000)
                .buffer(100)
                .parallel(8)
                .runOn(Schedulers.newParallel("yy"))
                .log()
                .flatMap(list -> Flux.fromIterable(list))
                .collectSortedList(Integer::compareTo)
                .subscribe(v -> System.out.println("v = " + v));
        System.in.read();
    }

    private static void context() {
        Flux.just(1, 2, 3)
                .transformDeferredContextual((flux, context) -> {
                    System.out.println("flux = " + flux);
                    System.out.println("context = " + context);
                    return flux.map(i -> i + "==>" + context.get("prefix"));
                })
                // 上游能拿到下游的最近一次数据,和 ThreadLocal 相反,ThreadLocal在响应式编程中无法使用,因为线程会频繁切换
                .contextWrite(Context.of("prefix", "前缀"))
                .subscribe(v -> System.out.println("v = " + v));
    }

    private static void block() {
        Integer block = Flux.just(1, 2, 3)
                .next()
                .block();
        System.out.println(block);
    }

    private static void onErrorContinue() {
        Flux.just(1, 2, 3, 0, 5)
                .map(i -> 10 / i)
                .onErrorContinue((err, val) -> {
                    System.out.println("err = " + err);
                    System.out.println("val = " + val);
                    System.out.println("发现" + val + "有问题了,继续执行其他的,我会记录这个问题");
                })
                .subscribe(v -> System.out.println("v = " + v),
                        err -> System.out.println("err = " + err));
    }

    private static void doFinally() {
        Flux.just(1, 2, 3, 4)
                .map(i -> "100 / " + i + " = " + (100 / i))
                .doOnError(err -> {
                    System.out.println("err已被记录 = " + err);
                })
                .doFinally(signalType -> {
                    System.out.println("最终操作：" + signalType);
                })
                .subscribe();
    }

    private static void doOnError2() {
        Flux.just(1, 2, 0, 4)
                .map(i -> "100 / " + i + " = " + (100 / i))
                .doOnError(err -> {
                    System.out.println("err已被记录 = " + err);
                }).subscribe(v -> System.out.println("v = " + v),
                        err -> System.out.println("err = " + err),
                        () -> System.out.println("流结束"));
    }

    private static void onErrorMap() {
        Flux.just(1, 2, 0, 4)
                .map(i -> "100 / " + i + " = " + (100 / i))
                .onErrorMap(err -> new RuntimeException(err.getMessage() + ": 又炸了..."))
                .subscribe(v -> System.out.println("v = " + v),
                        err -> System.out.println("err = " + err),
                        () -> System.out.println("流结束"));
    }

    private static void onErrorResume2() {
        Flux.just(1, 2, 0, 4)
                .map(i -> "100 / " + i + " = " + (100 / i))
                .onErrorResume(err -> Flux.error(new RuntimeException(err.getMessage() + ":炸了")))
                .subscribe(v -> System.out.println("v = " + v),
                        err -> System.out.println("err = " + err),
                        () -> System.out.println("流结束"));
    }

    private static void onErrorResume() {
        Flux.just(1, 2, 0, 4)
                .map(i -> "100 / " + i + " = " + (100 / i))
                .onErrorResume(err -> Mono.just("onErrorResume-"))
                .subscribe(v -> System.out.println("v = " + v),
                        err -> System.out.println("err = " + err),
                        () -> System.out.println("流结束"));
    }

    private static void onErrorReturn() {
        Flux.just(1, 2, 0, 4)
                .map(i -> "100 / " + i + " = " + (100 / i))
                .onErrorReturn(NullPointerException.class, "onErrorReturn-NullPointerException")
//                .onErrorReturn(ArithmeticException.class,"onErrorReturn-ArithmeticException")
                .subscribe(v -> System.out.println("v = " + v),
                        err -> System.out.println("err = " + err),
                        () -> System.out.println("流结束"));
    }

    private static void scheduler() {
        Scheduler s1 = Schedulers.newParallel("parallel-scheduler1", 4);
        Scheduler s2 = Schedulers.newParallel("parallel-scheduler2", 4);

        final Flux<String> flux = Flux
                .range(1, 2)
                .map(i -> 10 + i)
                .log()
                .publishOn(s1)
                .map(i -> "value " + i);

        flux.subscribeOn(s2).subscribe();

        // 不指定线程池,默认发布者用的线程就是订阅者的线程；
        new Thread(() -> flux.subscribe(System.out::println)).start();
    }

    private static void handle() {
        Flux.range(1, 10)
                .handle((value, sink) -> {
                    System.out.println("原始值：" + value);
                    sink.next("sink:" + value);
                })
                .log()
                .subscribe();
    }

    private static void limitRate() {
        Flux.range(1, 1000)
                .log()
                .limitRate(100) // 用完3/4就会取3/4.第一次request(100),以后request(75)
                .subscribe();
    }

    private static void buffer() {
        Flux.range(1, 10)
                .buffer(3)
                .log()
                .subscribe();
    }

    private static void baseSubscribe() {
        Flux<Integer> flux = Flux.just(1, 2, 3, 4, 5);
        flux.subscribe(new BaseSubscriber<Integer>() {
            @Override
            protected void hookOnSubscribe(Subscription subscription) {
                System.out.println("订阅关系绑定的时候触发-" + subscription);
                // 找发布者要数据
                request(1);
                // 要无限数据
//                requestUnbounded();
            }

            @Override
            protected void hookOnNext(Integer value) {
                System.out.println("数据到达,正在处理:" + value);
                request(1); //要1个数据
            }

            @Override
            protected void hookOnComplete() {
                System.out.println("流正常结束...");
            }

            @Override
            protected void hookOnError(Throwable throwable) {
                System.out.println("流异常..." + throwable);
            }

            @Override
            protected void hookOnCancel() {
                System.out.println("流被取消...");
            }

            @Override
            protected void hookFinally(SignalType type) {
                System.out.println("最终回调...一定会被执行");
            }
        });
    }

    private static void subscribe() {
        Flux<Integer> flux = Flux.just(1, 2, 3, 4, 5);
        flux.subscribe(
                v -> System.out.println("v = " + v),
                throwable -> System.out.println("异常 = " + throwable),
                () -> System.out.println("流结束了...")
        );
    }

    /**
     * 连接合并
     */
    private static void concat() {
        // concatMap 一个元素可以变很多单个,对于元素类型无限制
        // concat Flux.concat,静态调用
        // concatWith 连接的流和老流中的元素类型要一样
        Flux.concat(Flux.just(1, 2, 3), Flux.just(7, 8, 9))
                .subscribe(System.out::println);
    }

    /**
     * 执行顺序
     */
    private static void executionOrder() {
        Flux.range(1, 7)
                .log()
                .filter(i -> i > 3)
                .log()
                .map(i -> "map-" + i)
                .log()
                .subscribe(System.out::println);
    }

    /**
     * doOn 钩子(Hook)函数 流中某个元素到达以后触发我一个回调,卸载触发流的后面,新流的前面
     * 响应式编程核心：看懂文档弹珠图；
     * 信号： 正常/异常（取消）
     * SignalType：
     * SUBSCRIBE： 被订阅
     * REQUEST：  请求了N个元素
     * CANCEL： 流被取消
     * ON_SUBSCRIBE：在订阅时候
     * ON_NEXT： 在元素到达
     * ON_ERROR： 在流错误
     * ON_COMPLETE：在流正常完成时
     * AFTER_TERMINATE：中断以后
     * CURRENT_CONTEXT：当前上下文
     * ON_CONTEXT：感知上下文
     * <p>
     * doOnXxx API触发时机
     * 1、doOnNext：每个数据（流的数据）到达的时候触发
     * 2、doOnEach：每个元素（流的数据和信号）到达的时候触发
     * 3、doOnRequest： 消费者请求流元素的时候
     * 4、doOnError：流发生错误
     * 5、doOnSubscribe: 流被订阅的时候
     * 6、doOnTerminate： 发送取消/异常信号中断了流
     * 7、doOnCancle： 流被取消
     * 8、doOnDiscard：流中元素被忽略的时候
     */
    public static void doOnXxxx() {
        Flux.just(1, 2, 3, 4, 5, 6, 7, 0, 5, 6)
                .doOnNext(integer -> System.out.println("元素到达1：" + integer))
                .doOnEach(integerSignal -> {
                    System.out.println("doOnEach.." + integerSignal);
                })
                .map(integer -> 10 / integer)
                .doOnError(throwable -> {
                    System.out.println("数据库已经保存了异常：" + throwable.getMessage());
                })
                .map(integer -> 100 / integer)
                .doOnNext(integer -> System.out.println("元素到达2：" + integer))
                .subscribe(System.out::println);
    }

    public static void fluxSubscribeDoOn() throws Exception {
        // subscribe 事件回调,钩子函数(Hook),doOnXxx
        Flux<Integer> flux = Flux.range(1, 7)
                .delayElements(Duration.ofSeconds(1))
                .doOnComplete(() -> System.out.println("流正常结束..."))
                .doOnCancel(() -> System.out.println("流已被取消..."))
                .doOnError(throwable -> System.out.println("流出错..." + throwable))
                .doOnNext(integer -> System.out.println("doOnNext..." + integer));

        flux.subscribe(new BaseSubscriber<>() {
            @Override
            protected void hookOnSubscribe(Subscription subscription) {
                System.out.println("订阅者和发布者绑定好了：" + subscription);
                request(1);
            }

            @Override
            protected void hookOnNext(Integer value) {
                System.out.println("元素到达：" + value);
                if (value < 5) {
                    if (value == 3) {
                        int i = 10 / 0;
                    }
                    request(1);
                } else {
                    cancel();
                }
            }

            @Override
            protected void hookOnComplete() {
                System.out.println("数据流结束");
            }

            @Override
            protected void hookOnError(Throwable throwable) {
                System.out.println("数据流异常");
            }

            @Override
            protected void hookOnCancel() {
                System.out.println("数据流被取消");
            }

            @Override
            protected void hookFinally(SignalType type) {
                System.out.println("结束信号：" + type);
            }
        });

        System.in.read();
    }

    public static void flux() throws Exception {
        // Mono:0|1个元素的流, Flux:N个元素的流, Flux 有 empty 方法
        Flux<Integer> just = Flux.just(1, 2, 3, 4, 5); //

        // 流不消费就不执行,一个数据流可以有很多消费者,流是通过广播模式给消费者
        just.subscribe(e -> System.out.println("e1 = " + e));
        just.subscribe(e -> {
            System.out.println("e2 = " + e);
            try {
                Thread.sleep(1000);
            } catch (InterruptedException ex) {
                throw new RuntimeException(ex);
            }
        });

        System.out.println("=====每秒产生一个从0开始的递增数字=====");
        Flux<Long> flux = Flux.interval(Duration.ofSeconds(1));
        flux.subscribe(System.out::println);

        System.in.read();
    }
}
```

## 结尾

高并发 缓存 异步 队列

高可用 分片 复制 主从

以上就是本文核心内容.

[Github 源码](https://github.com/Awaion/tools/tree/master/demo025)

[返回顶部](#主要内容)

