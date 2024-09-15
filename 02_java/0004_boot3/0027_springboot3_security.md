# SpringBoot 3 + Spring Security

# 主要内容

> [安全](#安全)  
 
## 安全

常用安全框架 Apache Shiro,Spring Security,企业自研Filter

常见安全架构组件 认证(Authentication),授权(Authorization),攻击防护(XSS,CSRF,CORS,SQL注入)

权限模型 RBAC(Role Based Access Controll) 用户表,角色表,用户和角色表,权限表,角色和权限表

权限模型 ACL(Access Control List) 用户表,权限表,用户权限列表

Spring Security 基于 Servlet 的 Filter 思想,利用 FilterChainProxy 封装一系列拦截器链,实现各种安全拦截功能

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>

@Bean
SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    // 请求授权
    http.authorizeHttpRequests(registry -> {
        registry.requestMatchers("/").permitAll() // 首页所有人都允许
                .anyRequest().authenticated(); // 剩下的任意请求都需要认证
    });

    // 表单登录功能,开启默认表单登录功能, Spring Security 提供默认登录页
    http.formLogin(formLogin -> {
        formLogin.loginPage("/login").permitAll(); //自定义登录页位置，并且所有人都能访问
    });

    return http.build();
}

@Bean
UserDetailsService userDetailsService(PasswordEncoder passwordEncoder) {
    UserDetails zhangsan = User.withUsername("zhangsan")
            .password(passwordEncoder.encode("123456"))
            .roles("admin", "hr")
            .authorities("file_read", "file_write")
            .build();

    UserDetails lisi = User.withUsername("lisi")
            .password(passwordEncoder.encode("123456"))
            .roles("hr")
            .authorities("file_read")
            .build();

    UserDetails wangwu = User.withUsername("wangwu")
            .password(passwordEncoder.encode("123456"))
            .roles("admin")
            .authorities("file_write", "world_exec")
            .build();

    // 默认内存中保存所有用户信息
    InMemoryUserDetailsManager manager = new InMemoryUserDetailsManager(zhangsan, lisi, wangwu);
    return manager;
}

@Controller
public class LoginController {

    @GetMapping("/login")
    public String loginPage() {
        return "login";
    }
}
```

## 结尾

以上就是本文核心内容.

[Github 源码](https://github.com/Awaion/tools/tree/master/demo021)

[返回顶部](#主要内容)

