# Lambda + Stream API + Reactive Stream

# 主要内容

> [编程范式](#编程范式)  
> [Lambda 表达式](#lambda-表达式)  
> [Function 函数式接口](#function-函数式接口)  
> [Stream](#stream)  
> [Reactive Streams](#reactive-streams)  

## 编程范式

编程范式是一种基本的编程方法或风格,它为设计和实现计算机程序提供了一套原则,概念和技术,它定义了代码的结构,组织和流程,以及解决问题和表达计算的方法.

- 命令式编程(Imperative programming) 关注如何执行,将控制流定义为改变程序状态的语句.
  - 过程编程(Procedural programming): 指定程序达到所需状态必须采取的步骤.
  - 函数式编程(Functional programming): 将程序视为评估数学函数,避免状态和可变数据.
  - 面向对象编程(Object-oriented programming): 将程序组织为对象,由属性和方法及其交互组成的数据结构.
- 声明式编程(Declarative programming): 关注要执行的内容,定义程序逻辑,但不详细控制流程.
  - 事件驱动编程(Event-driven programming): 程序控制流由事件确定,例如传感器输入或用户动作(鼠标点击,按键)或来自其他程序或线程的消息.
  - 反应式编程(Reactive programming): 一种声明式编程范式,涉及数据流和变化的传播.

这些范式并不是相互排斥的,许多语言都包含了多个范式的元素,范式的选择取决于问题领域,语言能力以及程序员的个人偏好.

## Lambda 表达式

Lambda 表达式是一种匿名函数,也可称为闭包.它是没有声明的方法,也即没有访问修饰符,返回值声明和名字.

它可以写出更简洁,更灵活的代码.作为一种更紧凑的代码风格,使 Java 语言的表达能力得到了提升,性能上没变化.

Lambda 表达式核心原理是:能省则省

```text
// 函数式接口:自己创建实现类
System.out.println("函数式接口:自己创建实现类:" + new FunctionalInterface1Impl().sum(1, 2));

// 函数式接口:匿名函数实现完全写法
FunctionalInterface1 functionalInterface1 = new FunctionalInterface1() {
    @Override
    public int sum(int i, int j) {
        return i + j;
    }
};
System.out.println("函数式接口:匿名函数实现完全写法:" + functionalInterface1.sum(2, 3));

// 函数式接口:匿名函数 lambda 表达式写法
FunctionalInterface1 functionalInterface12 = (x, y) -> {
    System.out.println("lambda 表达式");
    return x + y;
};
System.out.println("函数式接口:匿名函数 lambda 表达式写法:" + functionalInterface12.sum(2, 3));

// 函数式接口:只有一行实现的极简写法
FunctionalInterface2 functionalInterface2 = () -> 1;
System.out.println("函数式接口:只有一行实现的极简写法:" + functionalInterface2.add());
System.out.println("函数式接口:只有一行实现的极简写法:不声明实现:" + ((FunctionalInterface2) () -> -1).subtract());

// 声明式函数式接口:规范检查
FunctionalInterface3 functionalInterface3 = (x, y) -> x * y;
System.out.println("声明式函数式接口:规范检查:" + functionalInterface3.multiply(4, 5));
System.out.println("声明式函数式接口:规范检查:不声明实现:" + ((FunctionalInterface3) (i, j) -> i * j).multiply(6, 7));

// Java8 语法糖:lambda 表达式:(参数表) -> {方法体}
// 函数式接口可以用 lambda 表达式

System.out.println("==============================================");

// 创建集合
var names = new ArrayList<String>();
names.add("Alice");
names.add("Bob");
names.add("Charlie");
names.add("David");

// 集合排序
names.sort(new Comparator<String>() {
    @Override
    public int compare(String o1, String o2) {
        return o2.compareTo(o1);
    }
});
System.out.println("排序后集合:" + names);


names.sort((o1, o2) -> o1.compareTo(o2));
System.out.println("排序后集合:lambda写法:" + names);

names.sort(String::compareTo);
System.out.println("排序后集合:调用默认定义方法:" + names);

System.out.println("==============================================");
```

最佳实战: 某个方法传入参数,这个参数实例是一个接口对象,且只定义了一个方法,就直接用 lambda 简化写法

## Function 函数式接口

函数式接口,接口中有且只有一个未实现的方法,这个接口就叫函数式接口,只要是函数式接口就可以用 Lambda 表达式简化,Function使用的编程范式是函数式编程

@FunctionalInterface 快速检查接口是否函数式接口

| 对象         | 描述    | 参数      | 接口方法      |
|------------|-------|---------|-----------|
| Runnable   | 普通函数  | 无入参,无出参 | 省略接口      |
| BiConsumer | 消费者   | 有入参,无出参 | 接口:accept |
| Supplier   | 提供者   | 无入参,有出参 | 接口:get    |
| Function   | 多功能函数 | 有入参,有出参 | 接口:apply  |

java.util.function 包下 function 分类, Consumer(消费者),Supplier(提供者),Predicate(断言)

```text
// 消费者 有入参,无出参
BiConsumer<String, String> consumer = (a, b) -> {
    System.out.println("消费者 有入参,无出参 " + a + "," + b);
};
// 调用
consumer.accept("q", "a");

// 多功能函数 有入参,有出参
Function<String, String> function = (String x) -> x.replace(" ", "").toUpperCase(Locale.ROOT);
System.out.println("多功能函数 有入参,有出参:" + function.apply("q a  W"));

// 多功能函数 有两个入参,有出参 传递两个以上参数要用对象
BiFunction<String, Integer, String> biFunction = (a, b) -> a + b;
System.out.println("多功能函数 有两个入参,有出参:" + biFunction.apply("q", 9));

// 断言
Predicate<String> predicate = StringUtil::isNullOrEmpty;
System.out.println(predicate.negate().test("qa z"));

// 两个参数断言 传递两个以上参数要用对象
BiPredicate<String, String> biPredicate = (a, b) -> StringUtil.isNullOrEmpty(a) && StringUtil.isNullOrEmpty(b);
System.out.println(biPredicate.test(" ", " "));

// 提供者 无入参,有出参
Supplier<String> supplier = () -> UUID.randomUUID().toString();
System.out.println("提供者 无入参,有出参:" + supplier.get());

// 普通函数 无入参,无出参
Runnable runnable = () -> System.out.println("这是一个线程");
runnable.run();
```

```text
/**
 * 使用函数式接口实现:判断奇偶数
 *
 * @param supplier
 */
private static void checkNumber(Supplier<String> supplier) {
    // 验证是否一个数字
    Predicate<String> isNumber = str -> str.matches("-?\\d+(\\.\\d+)?");
    if (isNumber.test(supplier.get())) {
        // 把字符串变成数字
        Function<String, Integer> change = Integer::parseInt;
        Integer apply = change.apply(supplier.get());

        // 打印数字
        Consumer<Integer> consumer = integer -> {
            if (integer % 2 == 0) {
                System.out.println("使用函数式接口实现:判断奇偶数:偶数：" + integer);
            } else {
                System.out.println("使用函数式接口实现:判断奇偶数:奇数：" + integer);
            }
        };
        consumer.accept(apply);
    } else {
        System.out.println("使用函数式接口实现:判断奇偶数:非法的数字");
    }
}

/**
 * 正常实现:判断奇偶数
 *
 * @param param
 */
private static void checkNumber2(String param) {
    boolean isNumeric = Pattern.matches("-?\\d+(\\.\\d+)?", param);
    if (isNumeric) {
        int parseInt = Integer.parseInt(param);
        if (parseInt % 2 == 0) {
            System.out.println("正常实现:判断奇偶数:偶数：" + parseInt);
        } else {
            System.out.println("正常实现:判断奇偶数:奇数：" + parseInt);
        }
    } else {
        System.out.println(":正常实现:判断奇偶数非法的数字");
    }
}
```

## Stream

Java Stream 的底层实现是基于 Java 内部的流式处理器,这些处理器被设计为在管道中处理数据,以提供更高的性能.Java Stream 被设计为无状态,这意味着在
流的整个操作过程中,并不会保存任何有关流中数据元素的状态.使用的编程范式是:声明式编程

Stream 所有数据和操作被组合成流管道,流管道构成：
- 一个数据源(可以是数组,集合,生成器函数,I/O管道)
- 零或多个中间操作(将一个流变形成另一个流)
- 一个终止操作(产生最终结果)

最佳实战: for循环处理数据的统一全部用 StreamAPI 进行替换

```text
List<Person> list = List.of(
        new Person("张 三", "男", 16),
        new Person("王 五", "女", 20),
        new Person("李 四", "男", 22),
        new Person("孙 七", "女", 15),
        new Person("钱 六儿", "女", 35));

// Stream API 处理方式
Stream<String> sorted = list.stream()
        .limit(7) // 限制条数
//                .parallel() // 并发执行,有状态数据会有线程安全问题,一般处理无状态数据
//                .filter(person -> person.age > 18) // 过滤条件
        .takeWhile(person -> person.age > 15) // 顺序过滤,不满足条件就直接退出
        .peek(person -> System.out.println("filter peek:" + person)) // 简单打印
        .map(person -> person.getName() + person.getAge()) // 取出结果
        .peek(s -> System.out.println("map peek:" + s)) // 打印取出结果
        .flatMap(ele -> { // 对每一个数据展开
            String[] s = ele.split(" ");
            return Arrays.stream(s);
        })
        .distinct() // 去重
        .sorted(String::compareTo); // 排序
System.out.println("lazy....终止方法不调用不会开始处理流");
System.out.println("Stream API:" + sorted.toList());
```

## Reactive Streams

响应式编程是一种面向数据流和变化传播的声明式编程范式,可以在编程语言中很方便地表达静态或动态的数据流,相关的计算模型会自动将变化的值通过数据流进行传播.

响应式系统特性:即时响应性,回弹性,弹性,消息驱动

响应式编程实现方式：
- 底层: 基于数据缓冲队列 + 消息驱动模型 + 异步回调机制
- 编码: 流式编程 + 链式调用 + 声明式API
- 效果: 优雅全异步 + 消息实时处理 + 高吞吐量 + 占用少量资源

Reactive Streams 是 JVM 面向流的库的标准和规范
- 处理可能无限数量的元素
- 有序
- 在组件之间异步传递元素
- 强制性非阻塞,背压模式

正压:数据的生产者给消费者压力

背压:生产者产生大量数据,队列缓冲将请求缓存起来

Java 9 Reactive Streams 提供的组件：
Publisher 发布者,产生数据流
Subscriber 订阅者,消费数据流
Subscription 订阅关系,订阅关系是发布者和订阅者之间的关键接口,订阅者通过订阅来表示对发布者产生的数据的兴趣,订阅者可以请求一定数量的元素,也可以取消订阅.
Processor 处理器,处理器是同时实现了发布者和订阅者接口的组件,它可以接收来自一个发布者的数据,进行处理,并将结果发布给下一个订阅者.处理器在Reactor
中充当中间环节,代表一个处理阶段,允许你在数据流中进行转换,过滤和其他操作.

```text
public static void main(String[] args) throws InterruptedException {
    // 发布订阅模型 观察者模式

    // 创建发布者
    SubmissionPublisher<String> publisher = new SubmissionPublisher<>();

    // 创建处理器
    MyProcessor myProcessor1 = new MyProcessor();
    MyProcessor myProcessor2 = new MyProcessor();
    MyProcessor myProcessor3 = new MyProcessor();

    // 创建订阅者
    Flow.Subscriber<String> mySubscriber1 = new MySubscriber();
    Flow.Subscriber<String> mySubscriber2 = new MySubscriber();

    // 发布者和处理器绑定
    publisher.subscribe(myProcessor1);
    // 处理器和处理器绑定
    myProcessor1.subscribe(myProcessor2);
    // 处理器和处理器绑定
    myProcessor2.subscribe(myProcessor3);

    // 处理器和订阅者绑定
    myProcessor3.subscribe(mySubscriber1);
    // 处理器和订阅者绑定
    myProcessor3.subscribe(mySubscriber2);

    for (int i = 0; i < 10; i++) {
        if (i == 5) {
//                publisher.closeExceptionally(new RuntimeException("5555"));
        } else {
            // 发布者发布信息
            publisher.submit("p-" + i);
        }
    }

    // 发布者通道关闭
    publisher.close();

    Thread.sleep(10000);
}

/**
 * 自定义处理器
 */
static class MyProcessor extends SubmissionPublisher<String> implements Flow.Processor<String, String> {

    private Flow.Subscription subscription;

    @Override
    public void onSubscribe(Flow.Subscription subscription) {
        // 处理器和订阅绑定
        this.subscription = subscription;
        // 获取订阅1个数据
        subscription.request(1);
        System.out.println("处理器和订阅绑定:" + this.hashCode());
    }

    @Override
    public void onNext(String item) {
        // 对数据进行处理
        item += ":处理";
        // 对处理后数据提交,给下一个处理器处理
        super.submit(item);
        // 获取订阅1个数据
        subscription.request(1);
        System.out.println("处理数据:" + this.hashCode() + ":" + item);
    }

    @Override
    public void onError(Throwable throwable) {
        System.out.println("处理器异常");
    }

    @Override
    public void onComplete() {
        System.out.println("处理器完成");
    }
}

/**
 * 自定义订阅者
 */
static class MySubscriber implements Flow.Subscriber<String> {

    private Flow.Subscription subscription;

    @Override
    public void onSubscribe(Flow.Subscription subscription) {
        // 订阅者和订阅绑定
        this.subscription = subscription;
        // 获取一个数据
        subscription.request(1);
        System.out.println("订阅者和订阅绑定:" + this.hashCode());
    }

    @Override //在下一个元素到达时； 执行这个回调；   接受到新数据
    public void onNext(String item) {
        System.out.println("线程:" + Thread.currentThread() + ":" + this.hashCode() + ":订阅者接收数据:" + item);
        if (item.equals("p-7")) {
            // 取消订阅
            subscription.cancel();
        } else {
            // 获取一个数据
            subscription.request(1);
        }
    }

    @Override
    public void onError(Throwable throwable) {
        System.out.println("线程:" + Thread.currentThread() + "订阅者错误:" + throwable);
    }

    @Override
    public void onComplete() {
        System.out.println("线程:" + Thread.currentThread() + "订阅者完成");
    }

}
```

官网: https://www.reactive-streams.org/

官网: https://reactivemanifesto.org/

## 结尾

以上就是本文核心内容.

[Github 源码](https://github.com/Awaion/tools/tree/master/demo024)

[返回顶部](#主要内容)

