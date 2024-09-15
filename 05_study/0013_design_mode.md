# Design Mode

## 设计模式

创建型模式,关注对象的创建过程,将对象的创建与使用相分离,来降低系统的耦合度,提高代码的可扩展性
- 单例模式(Singleton):确保一个类只有一个实例,并提供一个全局访问点.如:数据库连接池,线程池,配置管理类
- 工厂方法模式(Factory Method):定义一个用于创建对象的接口,让子类决定实例化哪一个类.如:ApplicationContextFactory
- 抽象工厂模式(Abstract Factory):提供一个创建一系列相关或相互依赖对象的接口,而无需指定它们的具体类.如:AbstractBeanFactory
- 建造者模式(Builder):将一个复杂对象的构建与他的表示相分离,使得同样的构建过程可以创建不同的表示.如:Stream
- 原型模式(Prototype):用原型实例指定创建对象的种类,并且通过拷贝这些原型来创建新的对象.如:AbstractPrototypeBasedTargetSource

结构型模式:主要关注类或对象的组合.通过继承,接口等方式,将类或对象组合成更大的结构,以实现新的功能.
- 适配器模式(Adapter):将一个类的接口转换成客户期望的另一个接口,使得原本因接口不兼容而无法工作的类可以一起工作.如:HandlerAdapter
- 桥接模式(Bridge):将抽象部分与它们的实现部分分离,以便二者可以独立地变化.如:BridgeMethodResolver
- 组合模式(Composite):将对象组合成树形结构以表示“整体-部分”层次结构.如:Disposable.Composite
- 装饰器模式(Decorator):动态地给对象添加一些额外的职责,而不需要修改其原始类的代码.如:FilterInputStream
- 外观模式(Facade):提供一个统一的接口,用于访问子系统中一群接口的功能.如:ApplicationContextFacade
- 享元模式(Flyweight):通过共享对象来减少内存使用量.如:Integer(-128~127)
- 代理模式(Proxy):为其他对象提供一个代理,以控制对这个对象的访问.如:TransactionInterceptor

行为型模式:主要关注对象之间的通信和交互.通过定义对象之间的职责和交互方式,以实现复杂的功能.
- 观察者模式(Observer):定义对象间一对多的依赖关系,当一个对象的状态发生改变时,所有依赖于它的对象都得到通知并更新.如:SpringBootContextLoaderListener
- 策略模式(Strategy):定义一系列的算法,把它们一个个封装起来,并使他们可以互相替换.如:StrategyConfig
- 模板方法模式(Template Method):定义一个操作中的算法的骨架,而将一些步骤延迟到子类中.如:JdbcTemplate
- 命令模式(Command):将一个请求封装为一个对象,从而使你可以用不同的请求对客户进行参数化.如:CommandClient
- 状态模式(State):允许对象在其内部状态改变时改变它的行为.如:CircuitBreaker
- 职责链模式(Chain of Responsibility):使多个对象都有机会处理请求,从而避免请求的送发者和接收者之间的耦合关系.如:FilterChain
- 中介者模式(Mediator):用一个中介对象封装一些列的对象交互.如:Socket
- 备忘录模式(Memento):在不破坏对象的前提下,捕获一个对象的内部状态,并在该对象之外保存这个状态.如:ProcedureCallMementoImpl

## 创建型模式

```text
public class SingletonDemo {
    public static void main(String[] args) {
        // 单例模式(Singleton):确保一个类只有一个实例,并提供一个全局访问点.
        
        // 懒汉式,双重检查锁定 Double-Checked Locking,调用才创建
        SingletonDemo1.getInstance();
        // 饿汉式,类加载就创建
        SingletonDemo2.getInstance();
    }
}

class SingletonDemo1 {
    // 可见性声明 volatile
    private static volatile SingletonDemo1 instance = null;

    // 私有的构造方法,防止外部类实例化
    private SingletonDemo1() {
    }

    // 提供一个静态的公有方法,用于获取 Singleton 实例
    public static SingletonDemo1 getInstance() {
        if (instance == null) { // 第一次检查 双重检查锁定 Double-Checked Locking
            synchronized (SingletonDemo1.class) { // 同步锁
                if (instance == null) { // 第二次检查
                    instance = new SingletonDemo1();
                }
            }
        }
        return instance;
    }
}

class SingletonDemo2 {
    // 可见性声明 volatile
    private static volatile SingletonDemo2 instance = new SingletonDemo2();

    // 私有的构造方法,防止外部类实例化
    private SingletonDemo2() {
    }

    // 提供一个静态的公有方法,用于获取 Singleton 实例
    public static SingletonDemo2 getInstance() {
        return instance;
    }
}
```

```text
public class FactoryDemo {
    public static void main(String[] args) {
        // 工厂方法模式(Factory Method):定义一个用于创建对象的接口,让子类决定实例化哪一个类.
        // 使用 FactoryA 创建产品A
        ProductFactory creatorA = new ProductFactoryA();
        creatorA.create().use();

        // 使用 FactoryB 创建产品B
        ProductFactory creatorB = new ProductFactoryB();
        creatorB.create().use();
    }
}

interface Product {
    void use();
}

class ProductA implements Product {
    @Override
    public void use() {
        System.out.println("使用具体产品A");
    }
}

class ProductB implements Product {
    @Override
    public void use() {
        System.out.println("使用具体产品B");
    }
}

abstract class ProductFactory {
    public abstract Product create();
}

class ProductFactoryA extends ProductFactory {
    @Override
    public Product create() {
        return new ProductA();
    }
}

class ProductFactoryB extends ProductFactory {
    @Override
    public Product create() {
        return new ProductB();
    }
}
```

```text
public class AbstractFactoryDemo {
    public static void main(String[] args) {
        // 抽象工厂模式(Abstract Factory):提供一个创建一系列相关或相互依赖对象的接口,而无需指定它们的具体类.
        AbstractFactory factory = new ProductFactory1();

        ProductC productC = factory.createProductC();
        ProductD productD = factory.createProductD();

        productC.use();
        productD.use();
    }
}

interface ProductC {
    void use();
}

class ProductC1 implements ProductC {
    @Override
    public void use() {
        System.out.println("使用具体产品C1");
    }
}

interface ProductD {
    void use();
}

class ProductD1 implements ProductD {
    @Override
    public void use() {
        System.out.println("使用具体产品D1");
    }
}

interface AbstractFactory {
    ProductC createProductC();

    ProductD createProductD();
}

class ProductFactory1 implements AbstractFactory {
    @Override
    public ProductC createProductC() {
        return new ProductC1();
    }

    @Override
    public ProductD createProductD() {
        return new ProductD1();
    }
}
```

```text
public class BuilderDemo {
    public static void main(String[] args) {
        // 建造者模式(Builder):将一个复杂对象的构建与他的表示相分离,使得同样的构建过程可以创建不同的表示.
        ProductE product = new ProductE.Builder()
                .setPartA("A部分")
                .setPartB("B部分")
                .setPartC("C部分")
                .build();

        System.out.println(product.getPartA());
        System.out.println(product.getPartB());
        System.out.println(product.getPartC());
    }
}

class ProductE {
    @Getter
    private String partA;
    @Getter
    private String partB;
    @Getter
    private String partC;

    private ProductE(Builder builder) {
        this.partA = builder.partA;
        this.partB = builder.partB;
        this.partC = builder.partC;
    }

    public static class Builder {
        private String partA;
        private String partB;
        private String partC;

        public Builder setPartA(String partA) {
            this.partA = partA;
            return this;
        }

        public Builder setPartB(String partB) {
            this.partB = partB;
            return this;
        }

        public Builder setPartC(String partC) {
            this.partC = partC;
            return this;
        }

        public ProductE build() {
            return new ProductE(this);
        }
    }
}
```

```text
public class PrototypeDemo {
    public static void main(String[] args) {
        // 原型模式(Prototype):用原型实例指定创建对象的种类,并且通过拷贝这些原型来创建新的对象.
        Prototype prototype = new RealPrototype("Initial State");
        Prototype clonedPrototype = prototype.clone();
        if (clonedPrototype instanceof RealPrototype) {
            ((RealPrototype) clonedPrototype).setSomeState("Cloned State");
        }
        System.out.println("Prototype: " + prototype);
        System.out.println("Cloned Prototype: " + clonedPrototype);
    }
}

interface Prototype {
    Prototype clone();
}

@ToString
class RealPrototype implements Prototype, Cloneable {
    @Setter
    private String someState;

    public RealPrototype(String someState) {
        this.someState = someState;
    }

    @Override
    public Prototype clone() {
        try {
            return (RealPrototype) super.clone();
        } catch (CloneNotSupportedException e) {
            throw new RuntimeException(e);
        }
    }
}
```

## 结构型模式

```text
public class AdapterDemo {
    public static void main(String[] args) {
        // 适配器模式(Adapter):将一个类的接口转换成客户期望的另一个接口,使得原本因接口不兼容而无法工作的类可以一起工作.
        Target target = new Adapter(new Adaptee());
        target.request();
    }
}

interface Target {
    void request();
}

class Adaptee {
    public void specificRequest() {
        System.out.println("Called specificRequest()");
    }
}

class Adapter implements Target {
    private Adaptee adaptee;

    public Adapter(Adaptee adaptee) {
        this.adaptee = adaptee;
    }

    @Override
    public void request() {
        adaptee.specificRequest();
    }
}
```

```text
public class BridgeDemo {
    public static void main(String[] args) {
        // 桥接模式(Bridge):将抽象部分与它们的实现部分分离,以便二者可以独立地变化.
        Implementor implementorA = new ConcreteImplementorA();
        Implementor implementorB = new ConcreteImplementorB();

        Abstraction abstractionA = new RefinedAbstractionA(implementorA);
        Abstraction abstractionB = new RefinedAbstractionB(implementorB);

        abstractionA.operation();
        abstractionB.operation();

        abstractionA.implementor = implementorB;
        abstractionA.operation();
    }
}

abstract class Abstraction {
    protected Implementor implementor;

    public Abstraction(Implementor implementor) {
        this.implementor = implementor;
    }

    public abstract void operation();

    protected void callImplementorMethod() {
        implementor.operationImpl();
    }
}

class RefinedAbstractionA extends Abstraction {
    public RefinedAbstractionA(Implementor implementor) {
        super(implementor);
    }

    @Override
    public void operation() {
        System.out.println("RefinedAbstractionA.operation()");
        callImplementorMethod();
    }
}

class RefinedAbstractionB extends Abstraction {
    public RefinedAbstractionB(Implementor implementor) {
        super(implementor);
    }

    @Override
    public void operation() {
        System.out.println("RefinedAbstractionB.operation()");
        callImplementorMethod();
    }
}

interface Implementor {
    void operationImpl();
}

class ConcreteImplementorA implements Implementor {
    @Override
    public void operationImpl() {
        System.out.println("ConcreteImplementorA.operationImpl()");
    }
}

class ConcreteImplementorB implements Implementor {
    @Override
    public void operationImpl() {
        System.out.println("ConcreteImplementorB.operationImpl()");
    }
}
```

```text
public class CompositeDemo {
    public static void main(String[] args) {
        // 组合模式(Composite):将对象组合成树形结构以表示“整体-部分”层次结构.
        Component leaf1 = new Leaf("Leaf 1");
        Component leaf2 = new Leaf("Leaf 2");

        Composite composite = new Composite("Composite 1");
        composite.add(leaf1);
        composite.add(leaf2);

        composite.operation();
    }
}

interface Component {
    void operation();
}

class Leaf implements Component {
    private String name;

    public Leaf(String name) {
        this.name = name;
    }

    @Override
    public void operation() {
        System.out.println("Leaf: " + name + " is operating.");
    }
}

class Composite implements Component {
    private List<Component> children = new ArrayList<>();
    private String name;

    public Composite(String name) {
        this.name = name;
    }

    @Override
    public void operation() {
        System.out.println("Composite: " + name + " is operating.");

        for (Component child : children) {
            child.operation();
        }
    }

    public void add(Component component) {
        children.add(component);
    }

    public void remove(Component component) {
        children.remove(component);
    }

    public List<Component> getChildren() {
        return children;
    }
}
```

```text
public class DecoratorDemo {
    public static void main(String[] args) {
        // 装饰器模式(Decorator):动态地给对象添加一些额外的职责,而不需要修改其原始类的代码.
        ComponentA component = new ConcreteComponent();

        ConcreteDecoratorA decoratorA = new ConcreteDecoratorA(component);
        decoratorA.operation();

        ConcreteDecoratorB decoratorB = new ConcreteDecoratorB(decoratorA);
        decoratorB.operation();
    }
}

interface ComponentA {
    void operation();
}

class ConcreteComponent implements ComponentA {
    @Override
    public void operation() {
        System.out.println("ConcreteComponent.operation()");
    }
}

abstract class Decorator implements ComponentA {
    protected ComponentA component;

    public Decorator(ComponentA component) {
        this.component = component;
    }

    @Override
    public void operation() {
        if (component != null) {
            component.operation();
        }
    }

    // 添加额外的功能
    public abstract void addedFunction();
}

class ConcreteDecoratorA extends Decorator {
    public ConcreteDecoratorA(ComponentA component) {
        super(component);
    }

    @Override
    public void operation() {
        super.operation(); // 调用组件的operation()
        addedFunction(); // 添加额外的功能
    }

    @Override
    public void addedFunction() {
        System.out.println("ConcreteDecoratorA.addedFunction()");
    }
}

class ConcreteDecoratorB extends Decorator {
    public ConcreteDecoratorB(ComponentA component) {
        super(component);
    }

    @Override
    public void operation() {
        super.operation(); // 调用组件的operation()
        addedFunction(); // 添加额外的功能
    }

    @Override
    public void addedFunction() {
        System.out.println("ConcreteDecoratorB.addedFunction()");
    }
}
```

```text
public class FacadeDemo {
    public static void main(String[] args) {
        // 外观模式(Facade):提供一个统一的接口,用于访问子系统中一群接口的功能.
        Facade facade = new Facade();
        facade.unifiedOperation();
    }
}

interface SubSystemA {
    void operationA();
}

class SubSystemAImpl implements SubSystemA {
    @Override
    public void operationA() {
        System.out.println("Subsystem A operation A");
    }
}

interface SubSystemB {
    void operationB();
}

class SubSystemBImpl implements SubSystemB {
    @Override
    public void operationB() {
        System.out.println("Subsystem B operation B");
    }
}

class Facade {
    private SubSystemA subSystemA;
    private SubSystemB subSystemB;

    public Facade() {
        this.subSystemA = new SubSystemAImpl();
        this.subSystemB = new SubSystemBImpl();
    }

    public void unifiedOperation() {
        subSystemA.operationA();
        subSystemB.operationB();
    }
}
```

```text
public class FlyweightDemo {
    public static void main(String[] args) {
        // 享元模式(Flyweight):通过共享对象来减少内存使用量.
        FlyweightFactory factory = new FlyweightFactory();

        Flyweight flyweightA = factory.getFlyweight("A");
        Flyweight flyweightB = factory.getFlyweight("B");
        Flyweight flyweightA2 = factory.getFlyweight("A");

        flyweightA.operation("X");
        flyweightB.operation("Y");
        flyweightA2.operation("Z");

        System.out.println(flyweightA == flyweightA2);
    }
}

interface Flyweight {
    void operation(String extrinsicState);
}

class ConcreteFlyweight implements Flyweight {
    private String intrinsicState; // 内部状态

    public ConcreteFlyweight(String intrinsicState) {
        this.intrinsicState = intrinsicState;
    }

    @Override
    public void operation(String extrinsicState) {
        System.out.println("Intrinsic: " + intrinsicState + ", Extrinsic: " + extrinsicState);
    }
}

class FlyweightFactory {
    private Map<String, Flyweight> flyweights = new HashMap<>();

    public Flyweight getFlyweight(String key) {
        Flyweight flyweight = flyweights.get(key);
        if (flyweight == null) {
            flyweight = new ConcreteFlyweight(key);
            flyweights.put(key, flyweight);
        }
        return flyweight;
    }
}
```

```text
public class ProxyDemo {
    public static void main(String[] args) {
        // 代理模式(Proxy):为其他对象提供一个代理,以控制对这个对象的访问.
        RealSubject realSubject = new RealSubject();

        MyInvocationHandler handler = new MyInvocationHandler(realSubject);

        Subject proxy = (Subject) Proxy.newProxyInstance(
                RealSubject.class.getClassLoader(), // 加载代理类的类加载器
                RealSubject.class.getInterfaces(),   // 代理类要实现的接口列表
                handler                              // 指派方法调用的调用处理器
        );

        proxy.request();
    }
}

interface Subject {
    void request();
}

class RealSubject implements Subject {
    @Override
    public void request() {
        System.out.println("Called by RealSubject");
    }
}

class MyInvocationHandler implements InvocationHandler {
    private Object target;

    public MyInvocationHandler(Object target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("Before method call");
        Object result = method.invoke(target, args);
        System.out.println("After method call");
        return result;
    }
}
```

## 行为型模式

```text
public class ObserverDemo {
    public static void main(String[] args) {
        // 观察者模式(Observer):定义对象间一对多的依赖关系,当一个对象的状态发生改变时,所有依赖于它的对象都得到通知并更新.
        ConcreteSubject subject = new ConcreteSubject();
        Observer observer1 = new ConcreteObserver("Observer 1");
        Observer observer2 = new ConcreteObserver("Observer 2");

        subject.registerObserver(observer1);
        subject.registerObserver(observer2);

        subject.setState("New state");

        subject.removeObserver(observer1);

        subject.setState("Another new state");
    }
}

interface Observer {
    void update(String message);
}

class ConcreteObserver implements Observer {
    private String name;

    public ConcreteObserver(String name) {
        this.name = name;
    }

    @Override
    public void update(String message) {
        System.out.println(name + " received message: " + message);
    }
}

interface SubjectA {
    void registerObserver(Observer observer);

    void removeObserver(Observer observer);

    void notifyObservers(String message);
}

class ConcreteSubject implements SubjectA {
    private List<Observer> observers = new ArrayList<>();
    private String state;

    @Override
    public void registerObserver(Observer observer) {
        observers.add(observer);
    }

    @Override
    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }

    @Override
    public void notifyObservers(String message) {
        for (Observer observer : observers) {
            observer.update(message);
        }
    }

    public void setState(String state) {
        this.state = state;
        notifyObservers("Subject state has changed to: " + state);
    }

    public String getState() {
        return state;
    }
}
```

```text
public class StrategyDemo {
    public static void main(String[] args) {
        // 策略模式(Strategy):定义一系列的算法,把它们一个个封装起来,并使他们可以互相替换.
        Strategy strategyA = new ConcreteStrategyA();
        Strategy strategyB = new ConcreteStrategyB();

        Context context = new Context(strategyA);
        context.executeStrategy();
        context.setStrategy(strategyB);
        context.executeStrategy();
    }
}

interface Strategy {
    void execute();
}

class ConcreteStrategyA implements Strategy {
    @Override
    public void execute() {
        System.out.println("Executing strategy A");
    }
}

class ConcreteStrategyB implements Strategy {
    @Override
    public void execute() {
        System.out.println("Executing strategy B");
    }
}

class Context {
    private Strategy strategy;

    public Context(Strategy strategy) {
        this.strategy = strategy;
    }

    public void executeStrategy() {
        strategy.execute();
    }

    public void setStrategy(Strategy strategy) {
        this.strategy = strategy;
    }
}
```

```text
public class TemplateMethodDemo {
    public static void main(String[] args) {
        // 模板方法模式(Template Method):定义一个操作中的算法的骨架,而将一些步骤延迟到子类中.
        AbstractClass abstractClass = new ConcreteClass();
        abstractClass.templateMethod();
    }
}

abstract class AbstractClass {
    public final void templateMethod() {
        specificMethod1();
        abstractMethod();
        hookMethod();
        specificMethod2();
    }

    protected void specificMethod1() {
        System.out.println("Executing specific method 1 in abstract class");
    }

    protected abstract void abstractMethod();

    protected void specificMethod2() {
        System.out.println("Executing specific method 2 in abstract class");
    }

    protected void hookMethod() {
        System.out.println("Executing hook method in abstract class (default implementation)");
    }
}

class ConcreteClass extends AbstractClass {
    @Override
    protected void abstractMethod() {
        System.out.println("Executing abstract method in concrete class");
    }

    @Override
    protected void hookMethod() {
        System.out.println("Executing hook method in concrete class (overridden)");
    }
}
```

```text
public class CommandDemo {
    public static void main(String[] args) {
        // 命令模式(Command):将一个请求封装为一个对象,从而使你可以用不同的请求对客户进行参数化.
        Receiver receiver = new Receiver();
        Command command = new ConcreteCommand(receiver);
        Invoker invoker = new Invoker(command);
        invoker.executeCommand();
        invoker.undoCommand();
    }
}

interface Command {
    void execute();

    void undo();
}

class ConcreteCommand implements Command {
    private Receiver receiver;

    public ConcreteCommand(Receiver receiver) {
        this.receiver = receiver;
    }

    @Override
    public void execute() {
        receiver.action();
    }

    @Override
    public void undo() {
        receiver.undoAction();
    }
}

class Receiver {
    public void action() {
        System.out.println("Command action executed.");
    }

    public void undoAction() {
        System.out.println("Command action undone.");
    }
}

class Invoker {
    private Command command;

    public Invoker(Command command) {
        this.command = command;
    }

    public void executeCommand() {
        command.execute();
    }

    public void undoCommand() {
        command.undo();
    }
}
```

```text
public class StateDemo {
    public static void main(String[] args) {
        // 状态模式(State):允许对象在其内部状态改变时改变它的行为.
        ContextB context = new ContextB();
        System.out.println("Initial state: StateA");
        context.request();
        context.setState(new StateB());
        System.out.println("Current state: StateB");
        context.request();
    }
}

class ContextB {
    private State state;

    public ContextB() {
        state = new StateA();
    }

    public void setState(State state) {
        this.state = state;
    }

    public void request() {
        state.handle();
    }
}

interface State {
    void handle();
}

class StateA implements State {
    @Override
    public void handle() {
        System.out.println("Handling request in state A");
        // 状态A可能改变上下文的状态
        // context.setState(new StateB());
    }
}

class StateB implements State {
    @Override
    public void handle() {
        System.out.println("Handling request in state B");
        // 状态B可能改变上下文的状态
        // context.setState(new StateA());
    }
}
```

```text
public class ChainOfResponsibilityDemo {
    public static void main(String[] args) {
        // 职责链模式(Chain of Responsibility):使多个对象都有机会处理请求,从而避免请求的送发者和接收者之间的耦合关系.
        Handler handlerA = new ConcreteHandlerA();
        Handler handlerB = new ConcreteHandlerB();
        handlerA.setNextHandler(handlerB);
        handlerA.handleRequest("A-type request");
        handlerA.handleRequest("B-type request");
        handlerA.handleRequest("C-type request");
    }
}

interface Handler {
    void handleRequest(String request);

    Handler getNextHandler();

    void setNextHandler(Handler handler);
}

class ConcreteHandler implements Handler {
    private Handler nextHandler;

    @Override
    public void handleRequest(String request) {
        if (canHandle(request)) {
            process(request);
        } else if (nextHandler != null) {
            nextHandler.handleRequest(request);
        } else {
            System.out.println("No handler available for request: " + request);
        }
    }

    protected boolean canHandle(String request) {
        return false;
    }

    protected void process(String request) {
        System.out.println("Processing request in " + this.getClass().getSimpleName() + ": " + request);
    }

    @Override
    public Handler getNextHandler() {
        return nextHandler;
    }

    @Override
    public void setNextHandler(Handler handler) {
        this.nextHandler = handler;
    }
}

class ConcreteHandlerA extends ConcreteHandler {
    @Override
    protected boolean canHandle(String request) {
        return request.startsWith("A");
    }
}

class ConcreteHandlerB extends ConcreteHandler {
    @Override
    protected boolean canHandle(String request) {
        return request.startsWith("B");
    }
}
```

```text
public class MediatorDemo {
    public static void main(String[] args) {
        // 中介者模式(Mediator):用一个中介对象封装一些列的对象交互.
        Mediator mediator = new ConcreteMediator();
        Colleague colleague1 = new ConcreteColleague();
        Colleague colleague2 = new ConcreteColleague();
        mediator.register(colleague1);
        mediator.register(colleague2);
        colleague1.send("Hello from Colleague 1!");
    }
}

interface Mediator {
    void register(Colleague colleague);

    void relay(Colleague colleague, String message);
}

interface Colleague {
    void send(String message);

    void receive(String message);

    void setMediator(Mediator mediator);

    Mediator getMediator();
}

class ConcreteMediator implements Mediator {
    private List<Colleague> colleagues = new ArrayList<>();

    @Override
    public void register(Colleague colleague) {
        colleagues.add(colleague);
        colleague.setMediator(this);
    }

    @Override
    public void relay(Colleague colleague, String message) {
        for (Colleague otherColleague : colleagues) {
            if (otherColleague != colleague) {
                otherColleague.receive(message);
            }
        }
    }
}

class ConcreteColleague implements Colleague {
    private Mediator mediator;

    @Override
    public void send(String message) {
        mediator.relay(this, message);
    }

    @Override
    public void receive(String message) {
        System.out.println("Colleague received: " + message);
    }

    @Override
    public void setMediator(Mediator mediator) {
        this.mediator = mediator;
    }

    @Override
    public Mediator getMediator() {
        return mediator;
    }
}
```

```text
public class MementoDemo {
    public static void main(String[] args) {
        // 备忘录模式(Memento):在不破坏对象的前提下,捕获一个对象的内部状态,并在该对象之外保存这个状态.
        Originator originator = new Originator();
        originator.setState("State #1");
        System.out.println("Initial State: " + originator.getState());

        Caretaker caretaker = new Caretaker();
        caretaker.addMemento(originator.createMemento());

        originator.setState("State #2");
        System.out.println("Current State: " + originator.getState());

        originator.restoreMemento(caretaker.getMemento(0));
        System.out.println("Restored State: " + originator.getState());
    }
}

class Originator {
    private String state;

    public void setState(String state) {
        this.state = state;
    }

    public String getState() {
        return state;
    }

    public Memento createMemento() {
        return new Memento(state);
    }

    public void restoreMemento(Memento memento) {
        this.state = memento.getState();
    }
}

class Memento {
    private String state;

    public Memento(String state) {
        this.state = state;
    }

    public String getState() {
        return state;
    }
}

class Caretaker {
    private List<Memento> mementos = new ArrayList<>();

    public void addMemento(Memento memento) {
        mementos.add(memento);
    }

    public Memento getMemento(int index) {
        return mementos.get(index);
    }
}
```

## 面向对象

- 面向对象分析 Object-Oriented Analysis OOA
- 面向对象设计 Object-Oriented Design OOD
- 面向对象编程 Object-Oriented Programming OOP

SOLID 原则

单一职责原则(Single Responsibility Principle, SRP):
- 核心思想:一个类应该只负责一个功能领域中的相应职责

开闭原则(Open-Closed Principle, OCP):
- 核心思想:一个对象对扩展开放,对修改关闭.

里氏替换原则(Liskov Substitution Principle, LSP):
- 核心思想:子类必须能够替换其父类.

接口隔离原则(Interface Segregation Principle, ISP):
- 核心思想:客户端不应该依赖它不需要的接口

依赖倒置原则(Dependence Inversion Principle, DIP):
- 核心思想:高层模块不应该依赖于低层模块,二者都应该依赖于抽象

迪米特法则(Law of Demeter, LOD) 或 最少知识原则(Least Knowledge Principle, LKP):
- 核心思想:一个软件实体应当对其他软件实体尽可能少的了解,低耦合

合成复用原则(Composite/Aggregate Reuse Principle, CARP):
- 核心思想:优先使用组合或者聚合关系复用,少用继承关系复用,高内聚

## 分析模式

- 分层模式:将系统分为不同的层次,如表示层,业务逻辑层,数据访问层等.
- 发布-订阅模式:用于实现消息的广播和订阅机制.
- 仓库模式:用于管理数据的存储和检索.

## 敏捷开发模式

- 迭代开发:将项目分解为一系列短周期的迭代,通常持续 1-4 周。在每个迭代结束时,交付可工作的软件增量。
- 快速反馈:通过频繁的沟通,演示和测试,获取客户和团队成员的反馈,以便及时调整和改进。
- 自组织团队:团队成员共同决策,自我管理,根据项目需求和个人能力灵活分配任务。
- 持续集成和持续部署:频繁地将代码集成到主分支,并能够快速,可靠地将软件部署到生产环境。
- 灵活应变:能够快速响应需求的变化,优先处理最重要和最有价值的功能。

实践

- Scrum:以短周期的迭代为核心,通常为 1-4 周。 有明确的角色,如产品负责人,Scrum 主管和开发团队。计划会议,评审会议和回顾会议
- Kanban:可视化的工作流程看板。 强调工作流程的顺畅流动。对工作项的优先级和状态进行直观展示。
- 极限编程（XP）:注重客户满意度和快速反馈。采用结对编程,测试驱动开发（TDD）,持续集成等实践。 强调简单设计和代码重构,以保持代码的简洁和可维护性。

## UML

Unified Modeling Language

特点
- 可视化:使复杂的系统易于理解和交流。
- 标准化:为开发人员提供了一种通用的语言,便于团队协作和沟通。
- 可扩展性:可以根据具体需求进行扩展和定制。

实践

- 静态结构图:如类图,对象图,构件图,部署图
- 动态行为图:如用例图,活动图,状态机图,顺序图,通信图

## 领域驱动设计

DDD(Domain-Driven Design)领域驱动设计是一种软件开发方法论,是针对软件核心复杂性应对之道,懂技术和不懂技术的都能参与,主要目标和特点包括
- 业务与技术的统一:深入理解业务领域,抽象出业务模型,然后在应用中贯穿地使用这些模型,使开发人员和业务专家能够更好地协同合作.
- 四层架构:用户接口层,应用层,领域层,基础层
- 核心原则:
  - 统一语言:开发人员和业务专家使用相同的术语,以避免沟通障碍和理解误差.
  - 明确边界:将领域划分为不同的限界上下文,每个上下文内有自己的模型和业务规则.
  - 聚焦核心领域:将精力集中在解决业务核心问题上,将非核心业务外包或简化.
  - 充血模型:将领域模型赋予丰富的行为和状态,使其能够自主执行业务操作.
- 领域模型:领域模型是DDD的核心工具,是对业务领域的抽象和建模,它由实体(Entity),值对象(Value Object),领域服务(Domain Service)等元素组成.领域
模型制定了业务领域的规则,行为和状态,并提供了对业务需求的有效表达和实现.
- 优势:DDD可以帮助团队更好地理解业务需求,创建更加灵活,可维护的代码(减缓系统老化),并更好地应对业务变化.DDD还可以提高开发效率,促进业务和技术之间的协作.

服务之间的调用,可以划分一个调用领域,把所有远程调用集成到一个类或若干个类

## 文档编写

- 技术文档:简介,快速入门,详细配置,扩展接口,原理实现
- 架构文档:背景,架构图,技术选型,模块

## 软件工程

- 软件生命周期:需求分析,设计,编码,测试,维护
- 发展:敏捷开发,云计算与微服务架构,自动化测试与持续集成/持续部署（CI/CD）,人工智能,移动应用,安全,开源

## 建立技术部门

开公司更多的是应该追逐利润,技术也应该服务于商业,所有不要过度在意技术的优劣,应该关注于实现,当前也要坚持技术长期主义,不要钻牛角尖.

- 第一阶段,招合适的人
- 第二阶段,购买合适服务器(物理机,云服务器)
- 第三阶段,技术选型,开发规范,统一环境,统一软件
- 第四阶段,知识积累,代码质量,数据监控
- 第五阶段,自动化,可扩展,响应式通知
- 第六阶段,长期主义

## 技术选型及使用

- 初始阶段 现有技术直接用
- 快速发展阶段 技术配置自定义及扩展
- 技术瓶颈阶段 改源码及创造

## 需求分发

领域划分,是否通用,是否合理,预估效益,分配开发

## 代码扩展

SPI机制,Proxy,仿写源代码实现,改源码

## 技术学习方式

官方文档,AI助力,更多的开源代码,自身的认知水平提高

