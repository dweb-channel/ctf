## CUPP-社工密码字典生成工具

弱密码可能非常短或仅使用字母数字字符，使解密变得简单。弱密码也可以是很容易被分析用户的人猜到的密码，例如生日、昵称、地址、宠物或亲戚的名字，或者常见的单词，例如上帝、爱情、金钱或密码

[github](https://github.com/Mebus/cupp)

    -h      this menu

    -i      Interactive questions for user password profiling

    -w      Use this option to profile existing dictionary,
            or WyD.pl output to make some pwnsauce :)

    -l      Download huge wordlists from repository

    -a      Parse default usernames and passwords directly from Alecto DB.
            Project Alecto uses purified databases of Phenoelit and CIRT which where merged and enhanced.

    -v      Version of the program



## shell

输入被攻击目标的姓、名、外号、生日、父母的名字、外号、生日、子女的名字、外号、生日等等一系列的信息。

```bash
cupp -i
```

完成生成后查看

```bash
cat -n test.txt | more
```

