---
layout: post
title:  "linux常用知识点"
date:   2024-02-28 15:29:00 +0800
featured-img: linux_and_shell
categories: linux_and_shell
---

## Special Variables

| type                 | example           | explanation                                                  |
| -------------------- | ----------------- | ------------------------------------------------------------ |
| Individual Arguments | $1, $2, and So On | $2, and all variables named as positive nonzero integers contain the values of the script parameters, or arguments. |
| Number of Arguments  | $#                | The $# variable holds the number of arguments passed to a script |
| All Arguments        | $@                | The $@ variable represents all of a script’s arguments       |
| Script Name          | $0                | The $0 variable holds the name of the script                 |
| Process ID           | $$                | The $$ variable holds the process ID of the shell.           |
| Exit Code            | $?                | The $? variable holds the exit code of the last command that the shell executed. When a Unix program finishes, it leaves an exit code, a numeric value also known as an error code or exit value, for the parent process that started the program. When the exit code is zero (0), it typically means that the program ran without a problem. However, if the program has an error, it usually exits with a number other than 0 (but not always, as you’ll see next). |

## Startup File

系统环境变量放在/etc/profile, 用户环境变量放在~/.bashrc, 修改环境变量可以通过修改这两个文件，并通过source生效。printenv或者set可以查看环境变量。

## 行律与驱动程序

**驱动程序**

- 不同的硬件需要不同的驱动程序
- 与行律模块的接口：上行和下行字符流

**行律的作用**

- 一行内字符的缓冲、回显与编辑，直到按下回车键
- 数据加工，如：将\n转化为\r\n
- 将Ctrl-C字符转化为中止进程运行的信号(signal)

**行律功能的调整**

- 程序中通过编程的方法

- 相关命令stty，例如:

  stty erase  ^H (有的终端的backspace或ctrl+H用不了，可以设置一下，使得backspace可以删掉一个字符)

  stty -a (可以把行律所有控制信息的状态打印出来)      

![Snipaste_2024-02-29_14-59-26](/assets/img/posts/linux_and_shell/Snipaste_2024-02-29_14-59-26.png)

## date：读取系统日期和时间

- 读取系统日期和时间，命令 date

  Wed Nov 7 21:09:16 CST 2018

- 可以根据需要定制输出格式

  `date "+%Y-%m-%d %H:%M:%S Day %j"`
  2018-11-07 21:09:54 Day 311
  `date "+%s"`
  1541596457

  *311指的是今天是今年的第311天，*
  *格式控制字符串：第一个字符必须为+号，%Y代表年号，%m代表月份，%M代表分钟。*
  *%s 秒坐标（从UTC1970开始），常用于计算时间间隔*
  *Linux命令往往有很多选项和复杂的功能，通过man date查阅联机手册*

- 通过NTP协议校对系统时间：命令 ntpdate

  ntpdate 0.pool.ntp.org (设置时间，必须root用户)
  ntpdate –q 0.pool.ntp.org (查询时间，普通用户也可以)

## cal：打印日历

- 用法

  cal
  cal year
  cal month year

- 用法举例

  cal 打印当前月份的日历
  cal 2020 打印2020年的日历
  cal 10 2019 打印 2019年10月份的日历
  cal 12 打印公元12年的日历

## bc：计算器

![Snipaste_2024-02-29_15-51-20](/assets/img/posts/linux_and_shell/Snipaste_2024-02-29_15-51-20.png)

## passwd：更换口令

![Snipaste_2024-02-29_15-54-38](/assets/img/posts/linux_and_shell/Snipaste_2024-02-29_15-54-38.png)

## 口令的设置与验证

![Snipaste_2024-02-29_15-56-23](/assets/img/posts/linux_and_shell/Snipaste_2024-02-29_15-56-23.png)

## who：确定有谁在系统中

![Snipaste_2024-02-29_16-05-09](/assets/img/posts/linux_and_shell/Snipaste_2024-02-29_16-05-09.png)

## uptime：已开机时间(年龄)

![Snipaste_2024-02-29_20-47-13](/assets/img/posts/linux_and_shell/Snipaste_2024-02-29_20-47-13.png)

## top：列出资源占用排名靠前的进程

![Snipaste_2024-02-29_20-48-55](/assets/img/posts/linux_and_shell/Snipaste_2024-02-29_20-48-55.png)

## 命令ps

![Snipaste_2024-02-29_20-51-47](/assets/img/posts/linux_and_shell/Snipaste_2024-02-29_20-51-47.png)

## 命令ps列出的进程属性

![Snipaste_2024-02-29_20-52-58](/assets/img/posts/linux_and_shell/Snipaste_2024-02-29_20-52-58.png)

## free：了解内存使用情况

![Snipaste_2024-02-29_20-53-49](/assets/img/posts/linux_and_shell/Snipaste_2024-02-29_20-53-49.png)

## vmstat：了解系统负载

![Snipaste_2024-02-29_20-57-26](/assets/img/posts/linux_and_shell/Snipaste_2024-02-29_20-57-26.png)

## more

| 指令或快捷键 | 效果                                                         |
| ------------ | ------------------------------------------------------------ |
| 空格         | 显示下一屏                                                   |
| 回车         | 上滚一行，当所感兴趣的段落内容正好处于当前屏的 尾部，另有一部分在下一页中时，可以连续按回车， 将感兴趣的部分滚动上来 |
| q            | (quit)退出程序，后面的内容不再显示                           |
| /*pattern*   | 搜索指定模式的字符串，模式描述用正则表达式                   |
| /            | 继续查找指定模式的字符串                                     |
| h            | (Help)帮助信息。打印more命令的所有功能列表                   |
| Ctrl-L       | 屏幕刷新                                                     |

## less

![Snipaste_2024-03-01_14-05-54](/assets/img/posts/linux_and_shell/Snipaste_2024-03-01_14-05-54.png)

## cat与od：列出文件内容

- 基本功能与命名

  **cat** concatenate:串结，文本格式打印 （选项-n：行号）
  **od** octal dump逐字节打印（-c, -t c, -t x1，-t d1, -t u1选项）

- 举例

  | 命令                    | 解释                                          |
  | ----------------------- | --------------------------------------------- |
  | cat tryl.c              | 命令行参数1个                                 |
  | cat –n shudu.c          | 带上`-n`可以在显示的时候带上行号              |
  | cat tryl.c tryx.c try.h | 命令行参数3个                                 |
  | cat >try                | 命令行参数=0个，从stdin获取数据，直到*ctrl-d* |

  | 命令                       | 解释                                         |
  | -------------------------- | -------------------------------------------- |
  | od –t x1 x.dat             | 以十六进制打印文件x.dat各字节                |
  | `od –t x1 x.dat | more`    | 以十六进制打印文件x.dat各字节                |
  | od –c /bin/bash            | 逐字符方式打印文件，遇到不可打印字符打印编码 |
  | `echo abcdABCD | od –t x1` | 十六进制显示字符的ASCII码                    |

## head与tail：显示文件的头部或者尾部

![](/assets/img/posts/linux_and_shell/Snipaste_2024-03-01_14-33-08.png)

## tee：三通

- 功能

  将从标准输入stdin得到的数据抄送到标准输出stdout显示，同时存入磁盘文件中

- 应用举例

  ```shell
  ./myap | tee myap.log
  ```

## wc：字计数（word count）

![Snipaste_2024-03-01_14-45-15](/assets/img/posts/linux_and_shell/Snipaste_2024-03-01_14-45-15.png)

## sort：对文件内容排序

- sort选项

  ![Snipaste_2024-03-01_14-56-36](/assets/img/posts/linux_and_shell/Snipaste_2024-03-01_14-56-36.png)

- 举例

  下面是使用sort排序的例子。列出当前目录下存储空间最大的10个文件。其中，ls 命令的-s选项(size)列出文件时，文件名前边的数字是文件大小，单位不是字节数，是“块” 数。不同的系统，1块的大小可能会不同，有的是512字节，有的是1024字节。

  ```shell
  ls -s | sort -n | tail -10
  ```

  ![Snipaste_2024-03-01_14-58-31](/assets/img/posts/linux_and_shell/Snipaste_2024-03-01_14-58-31.png)

## tr：翻译字符

- 用法

  **tr** *string1 string2*

  把标准输入拷贝到标准输出，*string1*中出现的字符替换为*string2*中的对应字符

- 例

  1. `cat telnos | tr UVX uvx`

  2. 用［］指定一个集合

     `cat report | tr '[a-z]' '[A-Z]'`

     将小写字母改为大写字母

  3. 用`\`加三个八进制数字(类似C语言)表示一字符

     `cat file1 | tr % '\012'`

     将`%`改为换行符,注意不要漏掉必需的单引号

## uniq：筛选文件中的重复行 

![Snipaste_2024-03-01_15-31-16](/assets/img/posts/linux_and_shell/Snipaste_2024-03-01_15-31-16.png)

![Snipaste_2024-03-01_15-25-06](/assets/img/posts/linux_and_shell/Snipaste_2024-03-01_15-25-06.png)

## 正则表达式

### 正则表达式的特殊字符（元字符）

![Snipaste_2024-03-02_15-04-52](/assets/img/posts/linux_and_shell/Snipaste_2024-03-02_15-04-52.png)

### 单字符正则表达式

![Snipaste_2024-03-02_15-07-46](/assets/img/posts/linux_and_shell/Snipaste_2024-03-02_15-07-46.png)

### 单字符正则表达式：定义集合

![Snipaste_2024-03-02_15-09-26](/assets/img/posts/linux_and_shell/Snipaste_2024-03-02_15-09-26.png)

![Snipaste_2024-03-02_15-14-09](/assets/img/posts/linux_and_shell/Snipaste_2024-03-02_15-14-09.png)

### 单字符正则表达式的组合

![Snipaste_2024-03-02_18-07-32](/assets/img/posts/linux_and_shell/Snipaste_2024-03-02_18-07-32.png)

![Snipaste_2024-03-02_18-18-44](/assets/img/posts/linux_and_shell/Snipaste_2024-03-02_18-18-44.png)

### 锚点：$与^

![Snipaste_2024-03-02_18-09-47](/assets/img/posts/linux_and_shell/Snipaste_2024-03-02_18-09-47.png)

### 正则表达式扩展

![Snipaste_2024-03-02_18-13-22](/assets/img/posts/linux_and_shell/Snipaste_2024-03-02_18-13-22.png)

### grep(Global regular expression print)在文件中查找字符串

- 语法

  `grep 模式 文件名列表`

- 举例

  ```shell
  grep O_RDWR *.h
  ps -ef | grep liang
  ls -l / | grep '^d' | wc –l
  grep '[0-9]*' shudu.c
  grep '[0-9][0-9]*' shudu.c
  ```

### grep/egrep/fgrep

![Snipaste_2024-03-02_18-38-27](/assets/img/posts/linux_and_shell/Snipaste_2024-03-02_18-38-27.png)

### grep/fgrep/egrep选项

![Snipaste_2024-03-02_18-40-25](/assets/img/posts/linux_and_shell/Snipaste_2024-03-02_18-40-25.png)

例：

```shell
ls -l | grep '^d'
```

找出当前路径下的所有文件夹

## sed：流编辑

- 用法

  **sed** '命令' 文件名列表
  **sed** `–e` '命令1' `–e` '命令2' `–e` '命令3' 文件名列表
  **sed** `-f` 命令文件 文件名列表

- 举例

  ![Snipaste_2024-03-02_22-05-18](/assets/img/posts/linux_and_shell/Snipaste_2024-03-02_22-05-18.png)

## sed:正则表达式替换

![Snipaste_2024-03-02_22-26-28](/assets/img/posts/linux_and_shell/Snipaste_2024-03-02_22-26-28.png)

## awk：逐行扫描进行文本处理的一门语言

![Snipaste_2024-03-03_17-04-46](/assets/img/posts/linux_and_shell/Snipaste_2024-03-03_17-04-46.png)

## awk描述条件的方法

- 使用与C语言类似的关系算符

  ![Snipaste_2024-03-03_17-09-01](/assets/img/posts/linux_and_shell/Snipaste_2024-03-03_17-09-01.png)

- 使用与C语言类似的逻辑算符

  ![Snipaste_2024-03-03_17-10-19](/assets/img/posts/linux_and_shell/Snipaste_2024-03-03_17-10-19.png)

- 正则表达式的模式匹配 /regexpr/

  包含该模式的行，执行动作

- 特殊的条件

  1. 不指定任何条件，对所有文本行执行动作
  2. BEGIN 开始处理所有文本行之前执行动作
  3. END 处理完所有文本行之后执行动作

## awk描述动作的方法

![Snipaste_2024-03-03_17-17-26](/assets/img/posts/linux_and_shell/Snipaste_2024-03-03_17-17-26.png)

![Snipaste_2024-03-03_21-53-33](/assets/img/posts/linux_and_shell/Snipaste_2024-03-03_21-53-33.png)

```shell
ps -ef | grep systemd
# 找出含有"systemd"的行，并只打印出第2列和第8列
ps -ef | awk '/systemd/{printf("%s %s\n", $2, $8); }'
# 只打印出时间
date | awk '{print $4}'
# 为源程序前每一行加上行号，花括号左边没有加任何条件，则每一行都匹配，NR表示记录号或行号，$0表示整一行的内容
awk '{printf("%d: %s\n",NR,$0);}' test.c
# ls -s列出当前目录下的文件及其大小，单位为1k，现在是挑出大于80k的文件进行打印
ls -s | tail -n +2 |  awk '$1 > 80 {print $2}'
```

## 应用实例

1. **以下是一个生成模拟的1000条nginx访问日志，并找出访问量前十的接口的shell脚本**

   ```shell
   #!/bin/bash
   
   # 定义日志文件的路径
   LOG_FILE="nginx_access.log"
   
   # 生成模拟的Nginx访问日志
   for i in {1..1000}; do
       # 随机选择一个资源
       resources=("/api/data" "/home" "/api/upload" "/login" "/user/82.png" "/login/37.php" "/data/5.jpg" "/user/5.html" "/api/48.css" "/logout/76.php" "/upload/41.html" "/upload/95.js" "/profile/41.css" "/info/94.css" "/upload/44.jpg" "/user/3.php" "/admin/7.xml" "/login/32.css" "/upload/79.css" "/upload/33.php" "/data/7.png" "/api/36.json" "/upload/23.xml" "/upload/21.jpg")
       resource=${resources[$RANDOM % ${#resources[@]}]}
       
       # 生成日志条目
       echo "192.168.0.1 - - [$(date +'%d/%b/%Y:%H:%M:%S +0000')] \"GET ${resource} HTTP/1.1\" 200 500 \"-\" \"Mozilla/5.0\"" >> $LOG_FILE
   done
   
   # 分析日志，找出访问量前10的资源
   echo "Top 10 accessed resources:"
   awk '{print $7}' $LOG_FILE | sort | uniq -c | sort -nr | head -10
   ```

   ![](/assets/img/posts/linux_and_shell/Snipaste_2024-03-01_16-33-02.png)

2. **累加作品发行量**

   ![Snipaste_2024-03-04_20-29-31](/assets/img/posts/linux_and_shell/Snipaste_2024-03-04_20-29-31.png)

   ![Snipaste_2024-03-04_21-06-08](/assets/img/posts/linux_and_shell/Snipaste_2024-03-04_21-06-08.png)

   ```shell
   sed -e 's/多//g' -e 's/.*[^0-9]\([0-9][0-9]*\) *万.*/\1/g' hh.txt | awk '/^[0-9]/{sum+=$1}; END {print sum}'
   ```

3. 求一本书中出现频率最高的20个单词

   ```shell
   # 查看单引号的八进制ASCII码值
   echo \' | od -t o1
   cat jane.txt | tr '[A-Z]' '[a-z]' | tr ';.?\047,():"!-' ' ' | tr ' ' '\012' | grep -v '^ *$' | sort | uniq -c | sort -n | tail -n 20
   ```

   ![Snipaste_2024-03-04_21-56-14](/assets/img/posts/linux_and_shell/Snipaste_2024-03-04_21-56-14.png)

## 常见问题

1. “死机”问题

   ![Snipaste_2024-03-06_20-19-20](/assets/img/posts/linux_and_shell/Snipaste_2024-03-06_20-19-20.png)

   网络虚拟终端的流控

   ![Snipaste_2024-03-06_20-21-46](/assets/img/posts/linux_and_shell/Snipaste_2024-03-06_20-21-46.png)

2. 意外中止问题

   ![Snipaste_2024-03-06_20-22-52](/assets/img/posts/linux_and_shell/Snipaste_2024-03-06_20-22-52.png)

3. 退格键(Backspace)无法使用的问题

   ![Snipaste_2024-03-06_20-25-11](/assets/img/posts/linux_and_shell/Snipaste_2024-03-06_20-25-11.png)

4. 屏幕显示乱码问题

   ![Snipaste_2024-03-06_20-28-49](/assets/img/posts/linux_and_shell/Snipaste_2024-03-06_20-28-49.png)

5. 文本文件格式问题

   ![Snipaste_2024-03-06_20-29-47](/assets/img/posts/linux_and_shell/Snipaste_2024-03-06_20-29-47.png)

   ![Snipaste_2024-03-06_20-32-14](/assets/img/posts/linux_and_shell/Snipaste_2024-03-06_20-32-14.png)

6. 中文编码问题

   ![Snipaste_2024-03-06_21-08-11](/assets/img/posts/linux_and_shell/Snipaste_2024-03-06_21-08-11.png)

## 目录

- /etc

  系统配置信息

  ![Snipaste_2024-03-06_21-17-33](/assets/img/posts/linux_and_shell/Snipaste_2024-03-06_21-17-33.png)

- /tmp

  临时文件，每个用户都可以在这里临时创建文件，但 只能删除自己的文件，不可以删除其他用户创建的文件

- /var

  系统运行时要改变的数据

  系统日志syslog等

- /bin

  系统常用命令，如ls，ln，cp，cat等

- /usr/bin

  存放一些常用命令，如ssh,ftp，make，gcc，git等

- /sbin,/usr/sbin

  系统管理员专用命令

- /dev

  设备文件，如终端设备，磁带机，打印机等

-  /usr/include (usr=Unix System Resource）

  C语言头文件存放目录

- /lib,/usr/lib

  ![Snipaste_2024-03-06_21-20-55](/assets/img/posts/linux_and_shell/Snipaste_2024-03-06_21-20-55.png)

## 文件通配符规则

**注意：** *文件名通配符规则与正则表达式的规则不同，应用场合不同*

- 星号 `*`

  ![Snipaste_2024-03-06_21-27-24](/assets/img/posts/linux_and_shell/Snipaste_2024-03-06_21-27-24.png)

- 问号 `?`

  匹配任一单字符

- 方括号 `[ ]`

  ![Snipaste_2024-03-06_21-28-06](/assets/img/posts/linux_and_shell/Snipaste_2024-03-06_21-28-06.png)

- 波浪线 `~`

  ![Snipaste_2024-03-06_21-28-47](/assets/img/posts/linux_and_shell/Snipaste_2024-03-06_21-28-47.png)

## shell与kernel

![Snipaste_2024-03-06_21-40-19](/assets/img/posts/linux_and_shell/Snipaste_2024-03-06_21-40-19.png)

## shell文件名通配符处理

文件名通配符的处理由shell完成，分以下三步

1. 在shell提示符下，从键盘输入命令，被shell接受
2. shell对所键入内容作若干加工处理，其中含有对文件通配符的展开工作 (文件名生成)，生成结果命令
3. 执行前面生成的结果命令

## 文件名通配符举例

![Snipaste_2024-03-06_21-50-03](/assets/img/posts/linux_and_shell/Snipaste_2024-03-06_21-50-03.png)

```c
#include <stdio.h>

int main(int argc, char *argv[])
{
    int i;
    for (i = 0; i < argc; i++)
        printf("%d:%p [%s]\n", i, argv[i], argv[i]);
}
```

   通过该c程序可以验证上面的内容，当然通过`set -x`也可以验证上面的内容

![Snipaste_2024-03-06_23-06-49](/assets/img/posts/linux_and_shell/Snipaste_2024-03-06_23-06-49.png)

## ls选项-l: 长格式列表

![Snipaste_2024-03-07_15-03-49](/assets/img/posts/linux_and_shell/Snipaste_2024-03-07_15-03-49.png)

- 第1列：文件属性

  ![Snipaste_2024-03-07_15-05-49](/assets/img/posts/linux_and_shell/Snipaste_2024-03-07_15-05-49.png)

- 第2列：文件link数，涉及到此文件的目录项数

- 第3列，第4列：文件主的名字和组名

- 第5列

  ![image-20240307150638941](/assets/img/posts/linux_and_shell/image-20240307150638941.png)

- 第6列：文件最后一次被修改的日期和时间

- 第7列：文件名(对于符号连接文件，附带列出符号连接文件的内容)

## ls的其他选项

![Snipaste_2024-03-07_15-09-20](/assets/img/posts/linux_and_shell/Snipaste_2024-03-07_15-09-20.png)

![Snipaste_2024-03-07_15-12-20](/assets/img/posts/linux_and_shell/Snipaste_2024-03-07_15-12-20.png)

## cp: 拷贝文件

![Snipaste_2024-03-07_15-22-39](/assets/img/posts/linux_and_shell/Snipaste_2024-03-07_15-22-39.png)

![Snipaste_2024-03-07_15-24-56](/assets/img/posts/linux_and_shell/Snipaste_2024-03-07_15-24-56.png)

## mv: 移动文件

![Snipaste_2024-03-07_16-15-18](/assets/img/posts/linux_and_shell/Snipaste_2024-03-07_16-15-18.png)

## rm: 删除文件

![Snipaste_2024-03-07_16-21-29](/assets/img/posts/linux_and_shell/Snipaste_2024-03-07_16-21-29.png)

![Snipaste_2024-03-07_16-22-28](/assets/img/posts/linux_and_shell/Snipaste_2024-03-07_16-22-28.png)

![Snipaste_2024-03-07_16-23-19](/assets/img/posts/linux_and_shell/Snipaste_2024-03-07_16-23-19.png)

当创建了一个名为"-i"的文件之后，执行`rm -i`是无法删除文件的，需要使用`rm -- -i`。

**注意:** cd是shell的一个内部命令，cd命令之后未给出任何参数，在DOS中,打印出当前工作目录;而在UNIX中,默认回到用户的主目录。

有必要在这里介绍一下“内部命令”和“外部命令”的概念，这里的内部和外部是相对于shell来说的。使用过DOS的用户都 熟悉“内部命令”和“外部命令”，例如：DIR，COPY，DEL等命令是内部命令，不需要磁盘上有一个类似DIR.COM或者DIR.EXE的程序文件，就可以直接执行DIR命令，而 XCOPY就是一个外部命令，必须在搜索路径PATH列出的某个子目录中有XCOPY.EXE文 件，才能执行。所谓的内部命令，就是输入了命令之后，直接由shell“内部”来理解的命令，不需要到系统中搜寻可执行程序文件。有的命令必须设计成内部命令，比如cd命令和umask命令都是内部命令。诸如ls，cp，rm等命令，在UNIX中都是外部命令。既然“内部命令”是由shell解释的，那么，内部命令与具体的shell相关， 不同的shell支持不同的内部命令集。外部命令没有这样的限制。有的shell为了提高执行效率，会把一些外部命令实现为同等语义的内部命令。例如：test，echo原来都是外部命令，许多shell把它们实现为内部命令。

## 创建/删除目录

![Snipaste_2024-03-07_22-24-29](/assets/img/posts/linux_and_shell/Snipaste_2024-03-07_22-24-29.png)

## cp: 复制目录

![Snipaste_2024-03-07_22-26-08](/assets/img/posts/linux_and_shell/Snipaste_2024-03-07_22-26-08.png)

![Snipaste_2024-03-08_17-15-51](/assets/img/posts/linux_and_shell/Snipaste_2024-03-08_17-15-51.png)

## rsync:数据备份工具（增量拷贝工具）

![Snipaste_2024-03-08_17-18-24](/assets/img/posts/linux_and_shell/Snipaste_2024-03-08_17-18-24.png)

## find:遍历目录树

![Snipaste_2024-03-08_17-22-18](/assets/img/posts/linux_and_shell/Snipaste_2024-03-08_17-22-18.png)

## find关于条件的选项

![Snipaste_2024-03-08_17-23-24](/assets/img/posts/linux_and_shell/Snipaste_2024-03-08_17-23-24.png)

![Snipaste_2024-03-08_17-24-12](/assets/img/posts/linux_and_shell/Snipaste_2024-03-08_17-24-12.png)

## find关于动作的选项

`-print`

打印查找的文件的路径名

`-exec`

- 对查找到的目标执行某一命令

- 在-exec及随后的分号之间的内容作为一条命令 在这命令的命令参数中，{}代表遍历到的目标文件的路径名

`-ok`

与-exec类似，只是对查找到符合条件的目标执行一个命令前需要经过操作员确认

## find使用举例

![Snipaste_2024-03-08_17-30-01](/assets/img/posts/linux_and_shell/Snipaste_2024-03-08_17-30-01.png)

![Snipaste_2024-03-08_17-30-34](/assets/img/posts/linux_and_shell/Snipaste_2024-03-08_17-30-34.png)

![Snipaste_2024-03-08_17-30-58](/assets/img/posts/linux_and_shell/Snipaste_2024-03-08_17-30-58.png)

```shell
find /lib /usr -name 'libc*.so' -exec ls -lh {} \;
```

- `-exec`及随后的分号之间的内容作为一条命令执行
- shell中分号有特殊含义，前面加反斜线`\`
- `{}`代表遍历时所查到的符合条件的路径名。注意，两花括号间无空格，之后的空格不可省略

-ok选项在执行指定的命令前等待用户确认

```shell
find ~ -size +100k \( -name core -o -name ‘*.tmp’ \) -ok rm {} \;
```

![Snipaste_2024-03-08_17-36-29](/assets/img/posts/linux_and_shell/Snipaste_2024-03-08_17-36-29.png)

加`/dev/null`是因为grep命令的特点，查找对象只有一个文件时，只列出行号，所以加一个`/dev/null`让grep认为有两个文件，从而使得grep打印的时候可以不仅打印出行号，同时打印出文件名。

## 问题

![Snipaste_2024-03-08_19-55-57](/assets/img/posts/linux_and_shell/Snipaste_2024-03-08_19-55-57.png)

## 利用find与xargs的组合

![Snipaste_2024-03-08_19-58-18](/assets/img/posts/linux_and_shell/Snipaste_2024-03-08_19-58-18.png)

## xargs

![Snipaste_2024-03-08_21-30-22](/assets/img/posts/linux_and_shell/Snipaste_2024-03-08_21-30-22.png)

![Snipaste_2024-03-08_21-31-10](/assets/img/posts/linux_and_shell/Snipaste_2024-03-08_21-31-10.png)
