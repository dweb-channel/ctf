## 文件包含漏洞

文件包含漏洞常见于 PHP 中，文件包含漏洞一般配合图片马进行直接连接，但是真正的挖洞情况下比较少遇到。

## 图片马

### 生成方式

- mac/linux

```bash
cat ./upload/file/php/shell_1.text >> ./upload/file/img/shell.jpeg
```

- Windows

```bash
copy shell.jpeg/b + shell_1.txt shell_1.jpeg
```

### 参考资料

[wgpsec](https://wiki.wgpsec.org/knowledge/web/fileincludes.html)
