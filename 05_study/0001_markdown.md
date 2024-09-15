# Markdown基本语法

# 主要内容

> [Markdown是什么](#Markdown是什么)  
> [为什么要使用它](#为什么要使用它)  
> [怎么用](#怎么用)  
>> [标题](#标题)  
>> [段落](#段落)  
>> [引用](#引用)  
>> [单行代码](#单行代码)  
>> [代码块](#代码块)  
>> [强调](#强调)  
>> [无序列表](#无序列表)  
>> [有序列表](#有序列表)  
>> [列表嵌套](#列表嵌套)  
>> [分割线](#分割线)  
>> [链接](#链接)  
>> [图片](#图片)  
>> [反斜杠](#反斜杠)  
>> [符号](#符号)  
>> [表格](#表格)  

# 正文

## Markdown是什么

**Markdown**是一种轻量级**标记语言**,它以纯文本形式编写文档,具有*易读,易写,易更改*特点,使普通文本内容具有一定的格式.      
由[**Aaron Swartz**](https://baike.baidu.com/item/%E4%BA%9A%E4%BC%A6%C2%B7%E6%96%AF%E6%B2%83%E8%8C%A8)和**John Gruber**共同设计.    

----

## 为什么要使用它

+ 易读(看起来舒服),易写(语法简单),易更改(纯文本).处处体现着**极简主义**的影子.
+ 兼容HTML,可以转换为HTML格式发布.
+ 很多网站支持Markdown编辑.

----

## 怎么用

Markdown语法主要分为如下几大部分:
**标题**,**段落**,**引用**,**代码**,**强调**,**列表**,**分割线**,**链接**,**图片**,**反斜杠 `\`**,**符号'`'**,**表格**.

----

### 标题

使用`#`,可表示1-6级标题.

```markdown
> # 一级标题   
> ## 二级标题   
> ### 三级标题   
> #### 四级标题   
> ##### 五级标题   
> ###### 六级标题   
```

效果:

> # 一级标题   
> ## 二级标题   
> ### 三级标题   
> #### 四级标题   
> ##### 五级标题   
> ###### 六级标题

----

### 段落

段落的前后要有空行,所谓的空行是指没有文字内容.若想在段内强制换行的方式是使用**两个以上**空格加上回车(引用中换行省略回车).

----

### 引用

在段落的每行或者只在第一行使用符号`>`,还可使用多个嵌套引用,如:

```markdown
> 区块引用  
>> 嵌套引用  
```

效果:

> 区块引用  
>> 嵌套引用

----

### 单行代码

代码之间分别用一个反引号`包起来   

```markdown
`Hello World` 
```   

效果:

`Hello World` 

----

### 代码块

代码之间分别用三个反引号`包起来  

````markdown
```
public static void main(String[] args) {
    SpringApplication.run(Demo006Application.class, args);
}
```
````

效果:

```
public static void main(String[] args) {
    SpringApplication.run(Demo006Application.class, args);
}
```

----

### 强调

在强调内容两侧分别加上`*`或者`_`,`~`表示删除,如:

```markdown
- *斜体*,_斜体_    
- **粗体**,__粗体__
- ***粗体斜体***,___粗体斜体___
- ~~删除线~~
```

效果:

- *斜体*,_斜体_    
- **粗体**,__粗体__
- ***粗体斜体***,___粗体斜体___
- ~~删除线~~

----

### 无序列表

使用`·`,`+`,或`-`标记无序列表,如:

```markdown
- 第一项
- 第二项
- 第三项
```

效果:

- 第一项
- 第二项
- 第三项

----

### 有序列表

有序列表的标记方式是数字,并辅以`.`,如:

```markdown
1. 第一项   
2. 第二项    
3. 第三项 
```  

效果:

1. 第一项   
2. 第二项    
3. 第三项 

----

### 列表嵌套

上一级和下一级之间敲三个空格即可

```markdown
1. 第一项
   1. 第一一项
   2. 第一二项
2. 第二项
3. 第三项
```  

效果:

1. 第一项
   1. 第一一项
   2. 第一二项
2. 第二项
3. 第三项

----

### 分割线

分割线最常使用就是三个或以上`*`,还可以使用`-`和`_`.

```markdown
***
----
_____
```

效果:

***
----
_____

### 链接

```markdown
[https://awaion.github.io/](https://awaion.github.io/ "Awaion")  
[https://awaion.github.io/](https://awaion.github.io/)
```   

效果:

[https://awaion.github.io/](https://awaion.github.io/ "Awaion")  
[https://awaion.github.io/](https://awaion.github.io/)

----

### 图片

添加图片的形式和链接相似,只需在链接的基础上前方加一个`！`.

```markdown
![Aaron_Swartz](./images/Aaron_Swartz.jpg)
```

效果

![Aaron_Swartz](./images/Aaron_Swartz.jpg)

----

### 反斜杠

相当于**反转义**作用.使符号成为普通符号.

```markdown
这是一个\+符号
```

效果:

这是一个\+符号

----

### 符号

起到标记作用.如:

```markdown
`ctrl+a`
```

效果:

`ctrl+a`

----

### 表格

```markdown
| 技术                 | 说明                | 官网                                           |
| -------------------- | ------------------- | ---------------------------------------------- |
| SpringBoot           | Web应用开发框架      | https://spring.io/projects/spring-boot         |
```

效果

| 技术                 | 说明                | 官网                                           |
| -------------------- | ------------------- | ---------------------------------------------- |
| SpringBoot           | Web应用开发框架      | https://spring.io/projects/spring-boot         |

----

### 注意 

不同的Markdown解释器或工具对相应语法(扩展语法)的解释效果不尽相同,具体可参见[Github # Markdown 语法](https://docs.github.com/zh/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

[返回顶部](#主要内容)

