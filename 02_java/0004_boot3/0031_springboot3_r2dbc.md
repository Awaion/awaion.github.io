# SpringBoot3 + R2DBC

Reactive Relational Database Connectivity(R2DBC) 项目将响应式编程 API 引入关系数据库

- r2dbc: https://r2dbc.io/
- spring-data-r2dbc: https://spring.io/projects/spring-data-r2dbc

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

spring.r2dbc.password=123456
spring.r2dbc.username=root
spring.r2dbc.url=r2dbc:mysql://localhost:3306/demo027?useUnicode=true&characterEncoding=utf8&serverTimezone=Asia/Shanghai&serverZoneId=Asia/Shanghai
spring.r2dbc.name=demo027

@SpringBootTest
class Demo027ApplicationTests {

    // 最佳实践
    // Spring Data R2DBC 基础的CRUD用 R2dbcRepository
    // 自定义复杂的单表SQL @Query
    // 多表查询复杂结果集 DatabaseClient 自定义SQL及结果封装

    // https://docs.spring.io/spring-data/relational/reference/r2dbc/entity-persistence.html
    @Autowired
    R2dbcEntityTemplate r2dbcEntityTemplate;

    @Autowired
    DatabaseClient databaseClient;

    @Autowired
    AuthorRepository authorRepository;

    @Autowired
    BookRepostory bookRepostory;

    @Autowired
    BookAuthorRepostory bookAuthorRepostory;

    @Autowired
    R2dbcCustomConversions r2dbcCustomConversions;

    @Test
    void oneToN() throws IOException {
        Flux<TAuthor> flux = databaseClient.sql("select a.id aid,a.name,b.* from t_author a left join t_book b on a.id = b.author_id order by a.id")
                .fetch()
                .all()
                .bufferUntilChanged(rowMap -> Long.parseLong(rowMap.get("aid").toString()))// 分组
                .map(list -> {
                    TAuthor tAuthor = new TAuthor();
                    Map<String, Object> map = list.get(0);
                    tAuthor.setId(Long.parseLong(map.get("aid").toString()));
                    tAuthor.setName(map.get("name").toString());
                    List<TBook> tBooks = list.stream()
                            .map(ele -> {
                                TBook tBook = new TBook();
                                tBook.setId(Long.parseLong(ele.get("id").toString()));
                                tBook.setAuthorId(Long.parseLong(ele.get("author_id").toString()));
                                tBook.setTitle(ele.get("title").toString());
                                return tBook;
                            })
                            .collect(Collectors.toList());

                    tAuthor.setBooks(tBooks);
                    return tAuthor;
                });
        flux.subscribe(tAuthor -> System.out.println("tAuthor = " + tAuthor));
        System.in.read();
    }

    @Test
    void author() throws IOException {
        authorRepository.findById(1L).subscribe(tAuthor -> System.out.println("tAuthor = " + tAuthor));
        authorRepository.findAll().subscribe(tAuthor -> System.out.println("tAuthor = " + tAuthor));
        System.in.read();
    }

    @Test
    void book() throws IOException {
        System.out.println("findById =>" + bookRepostory.findById(1L).block());
        System.out.println("bookInfo =>" + bookAuthorRepostory.bookInfo(1L).block());
        System.in.read();
    }

    @Test
    void databaseClient() throws IOException {
        databaseClient
                .sql("select * from t_author")
//                .bind(0,2L) // 绑定参数
                .fetch() // 抓取数据
                .all() // 返回所有
                .map(map -> { // 封装数据
                    System.out.println("map = " + map);
                    String id = map.get("id").toString();
                    String name = map.get("name").toString();
                    return new TAuthor(Long.parseLong(id), name, null);
                })
                .subscribe(tAuthor -> System.out.println("tAuthor = " + tAuthor));
        System.in.read();
    }

    @Test
    void r2dbcEntityTemplate() throws IOException {
        // JPA 语法
        Criteria criteria = Criteria
                .empty()
                .and("id").is(1L)
                .and("name").is("作者1");
        r2dbcEntityTemplate
                .select(Query.query(criteria), TAuthor.class)
                .subscribe(tAuthor -> System.out.println("tAuthor = " + tAuthor));
        System.in.read();
    }

    @Test
    void connection() throws IOException {
        MySqlConnectionConfiguration configuration = MySqlConnectionConfiguration.builder()
                .host("localhost")
                .port(3306)
                .username("root")
                .password("123456")
                .database("demo027")
                .serverZoneId(ZoneId.of("Asia/Shanghai"))
                .build();
        MySqlConnectionFactory connectionFactory = MySqlConnectionFactory.from(configuration);
        Mono.from(connectionFactory.create())
                .flatMapMany(connection ->
                        connection
                                .createStatement("select * from t_author where id=?id and name=?name")
                                .bind("id", 1L)
                                .bind("name", "作者1")
                                .execute()
                ).flatMap(result -> result.map(readable -> {
                    Long id = readable.get("id", Long.class);
                    String name = readable.get("name", String.class);
                    return new TAuthor(id, name, null);
                }))
                .subscribe(tAuthor -> System.out.println("tAuthor = " + tAuthor))
        ;
        System.in.read();
    }
}
```

## 结尾

以上就是本文核心内容.

[Github 源码](https://github.com/Awaion/tools/tree/master/demo027)

[返回顶部](#主要内容)

