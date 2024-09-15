# SpringBoot3 + R2DBC + WebFlux + Security

# 主要内容

SpringBoot Web开发响应式编程全栈

数据库层 spring-boot-starter-data-r2dbc: https://spring.io/projects/spring-data-r2dbc

web层 spring-boot-starter-webflux: https://docs.spring.io/spring-boot/reference/web/reactive.html

安全层 spring-boot-starter-security: https://docs.spring.io/spring-boot/reference/web/spring-security.html

```text
<dependency>
    <groupId>io.asyncer</groupId>
    <artifactId>r2dbc-mysql</artifactId>
    <version>1.0.5</version>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-r2dbc</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-webflux</artifactId>
</dependency>

spring.r2dbc.password=123456
spring.r2dbc.username=root
spring.r2dbc.url=r2dbc:mysql://localhost:3306/demo028?useUnicode=true&characterEncoding=utf8&serverTimezone=Asia/Shanghai&serverZoneId=Asia/Shanghai
spring.r2dbc.name=demo028

@Component
public class MyReactiveUserDetailsService implements ReactiveUserDetailsService {
    @Autowired
    DatabaseClient databaseClient;

    @Override
    public Mono<UserDetails> findByUsername(String username) {
        String sql = "SELECT t1.username AS `username`, t1.`password` AS `password`, t3.`value` AS `roleCode`, t5.`value` AS `permissionCode` FROM t_user t1 LEFT JOIN t_user_role t2 ON t2.user_id = t1.id LEFT JOIN t_roles t3 ON t3.id = t2.role_id LEFT JOIN t_role_perm t4 ON t4.role_id = t3.id LEFT JOIN t_perm t5 ON t4.perm_id = t5.id WHERE t1.username = ?";
        Mono<UserDetails> userDetailsMono = databaseClient.sql(sql)
                .bind(0, username)
                .fetch()
                .one()
                .map(map -> {
                    UserDetails details = User.builder()
                            .username(username)
                            .password(map.get("password").toString())
//                            .roles(map.get("roleCode").toString())
                            .authorities(map.get("permissionCode").toString()) // 源代码是新创建权限集合 AuthorityUtils.createAuthorityList
                            .build();
                    return details;
                });
        return userDetailsMono;
    }
}

@EnableR2dbcRepositories
@Configuration
public class MyR2DBCConfiguration {
}

@Configuration
@EnableReactiveMethodSecurity
public class MySecurityConfiguration {
    @Autowired
    ReactiveUserDetailsService appReactiveUserDetailsService;

    @Bean
    SecurityWebFilterChain springSecurityFilterChain(ServerHttpSecurity http) {
        http.authorizeExchange(authorize -> {
            authorize.matchers(PathRequest.toStaticResources()
                    .atCommonLocations()).permitAll();
            authorize.anyExchange().authenticated();
        });
        http.formLogin(formLoginSpec -> {
//            formLoginSpec.loginPage(""); // 指定登录页
        });
        http.csrf(ServerHttpSecurity.CsrfSpec::disable);
        http.authenticationManager(
                new UserDetailsRepositoryReactiveAuthenticationManager(
                        appReactiveUserDetailsService)
        );
        return http.build();
    }

    @Bean
    PasswordEncoder passwordEncoder() {
        return PasswordEncoderFactories.createDelegatingPasswordEncoder();
    }
}
```

## 结尾

以上就是本文核心内容.

[Github 源码](https://github.com/Awaion/tools/tree/master/demo028)

[返回顶部](#主要内容)

