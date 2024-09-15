# SpringBoot3 + WebFlux

# 主要内容

> [组件对比](#组件对比)  
> [Web Flux 核心对象](#web-flux-核心对象)  
> [请求参数](#请求参数)  
> [返回值](#返回值)  
> [文件上传](#文件上传)  
> [错误处理](#错误处理)  
> [请求上下文](#请求上下文)  
> [自定义配置](#自定义配置)  
> [过滤器](#过滤器)  

## 组件对比

| API功能    | Servlet-阻塞式Web                 | WebFlux-响应式Web                                         |
|----------|--------------------------------|--------------------------------------------------------|
| 前端控制器    | DispatcherServlet              | DispatcherHandler                                      |
| 处理器      | Controller                     | WebHandler/Controller                                  |
| 请求,响应    | ServletRequest,ServletResponse | ServerWebExchange:ServerHttpRequest,ServerHttpResponse |
| 过滤器      | Filter（HttpFilter）             | WebFilter                                              |
| 异常处理器    | HandlerExceptionResolver       | DispatchExceptionHandler                               |
| Web配置    | @EnableWebMvc                  | @EnableWebFlux                                         |
| 自定义配置    | WebMvcConfigurer               | WebFluxConfigurer                                      |
| 返回结果     | 任意                             | Mono,Flux,任意                                           
| 发送REST请求 | RestTemplate                   | WebClient                                              |

![001.png](0030_springboot3_webflux/001.png)

命令式编程 浏览器 --> Controller --> Service --> Dao(数据源)

响应式编程 Dao(发布者) --> Service --> Controller --> 浏览器

https://docs.spring.io/spring-boot/reference/web/reactive.html

https://docs.spring.io/spring-framework/reference/web/webflux.html

## Web Flux 核心对象

DispatcherHandler 处理流程
- HandlerMapping 请求映射处理器,保存每个请求由哪个方法进行处理
- HandlerAdapter 处理器适配器,反射执行目标方法
- HandlerResultHandler 处理器结果处理器

SpringMVC DispatcherServlet.doDispatch() 处理所有请求

WebFlux DispatcherHandler.handle() 处理所有请求

```text
public Mono<Void> handle(ServerWebExchange exchange) { 
    if (this.handlerMappings == null) {
        return createNotFoundError();
    }
    if (CorsUtils.isPreFlightRequest(exchange.getRequest())) {
        return handlePreFlight(exchange);
    }
    return Flux.fromIterable(this.handlerMappings) // 拿到所有的url映射方法键值对
            .concatMap(mapping -> mapping.getHandler(exchange)) // 找对应url方法
            .next() // 直接触发获取元素,拿到流的第一个元素,找到第一个能处理这个请求的 handlerAdapter,执行方法
            .switchIfEmpty(createNotFoundError()) // 如果没拿到这个元素,则响应404错误；
            .onErrorResume(ex -> handleDispatchError(exchange, ex)) // 异常处理,一旦前面发生异常,调用处理异常
            .flatMap(handler -> handleRequestWith(exchange, handler)); // 调用方法处理请求,得到响应结果
}

handleRequestWith 请求信息转化
handleResult 响应结果转化 String,User,ServerSendEvent,Mono,Flux
```

## 请求参数

https://docs.spring.io/spring-framework/reference/6.0/web/webflux/controller/ann-methods/arguments.html

| 请求参数                                                                              | 描述                                                     |
|-----------------------------------------------------------------------------------|--------------------------------------------------------|
| ServerWebExchange                                                                 | 封装了请求和响应对象的对象,自定义获取数据,自定义响应                            |
| ServerHttpRequest, ServerHttpResponse                                             | 请求,响应                                                  |
| WebSession                                                                        | 访问Session对象                                            |
| java.security.Principal                                                           | 安全                                                     |
| org.springframework.http.HttpMethod                                               | 请求方式                                                   |
| java.util.Locale                                                                  | 国际化                                                    |
| java.util.TimeZone + java.time.ZoneId                                             | 时区                                                     |
| @PathVariable                                                                     | 路径变量                                                   |
| @MatrixVariable                                                                   | 矩阵变量                                                   |
| @RequestParam                                                                     | 请求参数                                                   |
| @RequestHeader                                                                    | 请求头                                                    |
| @CookieValue                                                                      | 获取Cookie                                               |
| @RequestBody                                                                      | 获取请求体,Post,文件上传                                        |
| HttpEntity<B>                                                                     | 封装后的请求对象                                               |
| @RequestPart                                                                      | 获取文件上传的数据 multipart/form-data.                         |
| java.util.Map, org.springframework.ui.Model, and org.springframework.ui.ModelMap. | Map,Model,ModelMap                                     |
| @ModelAttribute                                                                   | 模型属性                                                   |
| Errors, BindingResult                                                             | 数据校验,封装错误                                              |
| SessionStatus + class-level @SessionAttributes                                    | 	会话                                                    |
| UriComponentsBuilder                                                              | URI建造者                                                 |
| @SessionAttribute                                                                 | 会话属性	                                                  |
| @RequestAttribute                                                                 | 	转发请求的请求域数据                                            |
| Any other argument                                                                | 	其它参数:基本类型,等于标注@RequestParam;对象类型,等于标注 @ModelAttribute |

## 返回值

SSE 和 WebSocket 区别
- SSE 请求过去以后,等待服务端源源不断的数据
- WebSocket 连接建立后,可以任何交互

| 响应值                                                                        | 描述                                       |
|----------------------------------------------------------------------------|------------------------------------------|
| @ResponseBody                                                              | 把响应数据写出去,如果是对象,可以自动转为json                |
| HttpEntity<B>, ResponseEntity<B>                                           | HttpEntity响应对象,ResponseEntity自定义响应对象     |
| HttpHeaders                                                                | 响应头                                      |
| ErrorResponse                                                              | 快速构建错误响应                                 |
| ProblemDetail                                                              | SpringBoot3提高错误响应对象                      |
| String                                                                     | forward:转发到一个地址;redirect:重定向到一个地址,配合模板引擎 |
| View                                                                       | 视图对象                                     |
| java.util.Map, org.springframework.ui.Model                                | 数据模型                                     |
| @ModelAttribute                                                            | 数据模型属性                                   |
| Rendering                                                                  | 新版的页面跳转API,不能标注@ResponseBody 注解          |
| void                                                                       | 仅代表响应完成信号                                |
| Flux<ServerSentEvent>, Observable<ServerSentEvent>, or other reactive type | 使用 text/event-stream 完成SSE效果             |
| Other return values                                                        | 未在上述列表的其他返回值,都会当成给页面的数据                  |

```text
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-webflux</artifactId>
</dependency>

@GetMapping("/hello")
@ResponseBody
public String hello(@RequestParam(value = "param", required = false, defaultValue = "defaultValue") String param) {
    // WebFlux 向下兼容原来 SpringMVC 的大多数注解和API
    // 可接受的请求参数
    // ServerWebExchange exchange, exchange.getRequest();exchange.getResponse();
    // WebSession webSession, webSession.getAttribute("");webSession.getAttributes().put("", "");
    // HttpMethod method, method.name();
    // HttpEntity<String> entity,
    // @RequestBody String s,
    // FilePart file, file.transferTo() // 零拷贝技术

    return "Hello World param=" + param;
}

@GetMapping("/render")
public Rendering render() {
//        Rendering.redirectTo("/"); // 重定向到当前项目根路径
    return Rendering.redirectTo("https://www.baidu.com").build();
}

@GetMapping("/mono")
@ResponseBody
public Mono<String> mono() {
    // 返回单个数据Mono
    return Mono.just(0)
            .map(i -> 10 / i)
            .map(i -> "mono-" + i);
}

@GetMapping("/flux")
@ResponseBody
public Flux<String> flux() {
    // 返回多个数据Flux
    return Flux.just("flux1", "flux2");
}

@GetMapping(value = "/sse", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
@ResponseBody
public Flux<ServerSentEvent<String>> sse() {
    // Flux + SSE(Server Send Event) 服务端事件推送
    return Flux.range(1, 10)
            .map(i -> ServerSentEvent.builder("sse-" + i)
                    .id(i + "")
                    .comment("comment-" + i)
                    .event("event")
                    .build())
            .delayElements(Duration.ofMillis(500));
}
```

## 文件上传

https://docs.spring.io/spring-framework/reference/6.0/web/webflux/controller/ann-methods/multipart-forms.html

```text
@PostMapping("/")
public String handle(@RequestPart("meta-data") Part metadata, 
		@RequestPart("file-data") FilePart file) { 
}
```

## 错误处理

```text
@RestControllerAdvice
public class MyGlobalExceptionHandler {

    @ExceptionHandler(RuntimeException.class)
    public ProblemDetail error(RuntimeException exception) {
        // 错误对象 ProblemDetail,ErrorResponse
        return ProblemDetail.forStatus(HttpStatus.INTERNAL_SERVER_ERROR.value());
    }
}
```

## 请求上下文

RequestContext

## 自定义配置

```text
@Configuration
public class MyWebConfiguration {
    @Bean
    public WebFluxConfigurer webFluxConfigurer() {
        return new WebFluxConfigurer() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                // 允许跨域
                registry.addMapping("/**")
                        .allowedHeaders("*")
                        .allowedMethods("*")
                        .allowedOrigins("*");
            }
        };
    }
}
```

## 过滤器

```text
@Component
public class MyWebFilter implements WebFilter {
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, WebFilterChain chain) {
        // 请求到controller之前,使用该过滤器
        Mono<Void> filter = chain.filter(exchange);

        // 流一旦经过某个操作就会变成新流
        return filter
                .doOnError(err -> System.out.println("目标方法异常以后..."))
                .doFinally(signalType -> System.out.println("目标方法执行以后..."));
    }
}
```

## 结尾

以上就是本文核心内容.

[Github 源码](https://github.com/Awaion/tools/tree/master/demo026)

[返回顶部](#主要内容)

