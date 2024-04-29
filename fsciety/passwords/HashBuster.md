## Hash-Buster

[github](https://github.com/s0md3v/Hash-Buster)

哈希克星！

## shell

### 破解单个

```bash
buster -s <hash>
```

### 从目录中查找哈希值

只需指定一个目录，Hash Buster 就会遍历其中存在的所有文件和目录，寻找哈希值。

```bash
从目录中查找哈希值
```

### 破解文件中的哈希值

```bash
buster -f /root/hashes.txt
```

### 指定线程数

当您需要通过并行发出请求来破解大量哈希值时，多线程可以极大地降低整体速度。

```bash
buster -f /root/hashes.txt -t 10
```
