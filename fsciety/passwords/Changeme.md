## Changeme-凭证扫描仪

Changeme 支持 http/https、mssql、mysql、postgres、ssh、ssh w/key、snmp、mongodb 和 ftp 协议。用于`./changeme.py --dump`输出所有当前可用的凭据。

### examples

- 扫描单个主机：./changeme.py 192.168.59.100
- 扫描子网中的默认信用：./changeme.py 192.168.59.0/24
- 使用 nmap 文件进行扫描./changeme.py subnet.xml
- 扫描子网中的 Tomcat 默认信用并将超时设置为 5 秒：./changeme.py -n "Apache Tomcat" --timeout 5 192.168.59.0/24
- 使用 Shodan 填充目标列表并检查它们的默认凭据：./changeme.py --shodan_query "Server: SQ-WEBCAM" --shodan_key keygoeshere -c camera
- 扫描 SSH 和已知的 SSH 密钥：./changeme.py --protocols ssh,ssh_key 192.168.59.0/24
- 使用协议语法扫描主机以获取 SNMP 信用信息：./changeme.py snmp://192.168.1.20

### 使用代理

通过指定--proxy 选项，changeme 能够通过代理转发其 http 流量。

```bash
./changeme.py --proxy http://192.168.59.3:8080 -v 192.168.59.3
```

### 指定端口

例如，扫描 Apache Tomcat 时，仅检查端口 8080。该--portoverride 标志可用于扫描替代端口。

```bash
./changeme.py -v -n  tomcat --portoverride 192.168.59.103:8180
```

### 指定协议

默认情况下 changeme 只扫描 http 协议。您可以使用 --protocol 选项来覆盖它，该选项采用逗号分隔的列表。

```bash
./changeme.py -v --protocols ssh,ssh_key 192.168.1.47
```
