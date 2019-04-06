---
layout: post
title:  "BitTorrent"
date:   2019-03-24 15:29:00 +0800
featured-img: bittorrent
categories: bittorrent
---

根据官方文档上的介绍，bittorrent文件分发由以下几部分组成。

- An ordinary web server
- A static 'metainfo' file
- A BitTorrent tracker
- An 'original' downloader
- The end user web browsers
- The end user downloaders

为了学习BitTorrent协议，我首先在网上下载了一个‘metainfo’文件进行分析。(the metainfo file: [engineering_education.torrent](/assets/file/bittorrent/engineering_education.torrent))

metafile(also known as .torrent file) 通过bencoding方式进行编码。对以上metainfo文件进行解析之后的结果如下：

```json
{'announce': 'https://academictorrents.com/announce.php',
 'announce-list': [['https://academictorrents.com/announce.php'],
                   ['udp://tracker.coppersurfer.tk:6969'],
                   ['udp://tracker.opentrackr.org:1337/announce'],
                   ['http://tracker.amazonaws.com:6969/announce']],
 'info': {'length': 3938107,
          'name': 'devinberg.com_ASEE_AC2014-8679_final.pdf',
          'piece length': 262144,
          'pieces': ['e466b5bdf563a96b2192e25e1fff8584264e8df4',
                     '7ae215d00db4dbb37a7dfc1e0be0084cce72eebd',
                     'f0868ec1a75361ae9c224baf4936af96ab1b5d2d',
                     '4ab67468269a0c1ad59bee4161750d57f5097df3',
                     'ecd167f43219abf2b0b4731a4ec29627779422fb',
                     '9081d00df42f00b8a3bb64224fca603d69eea67a',
                     '04457f2232c713b3491123ab3b557bc7be80dbbb',
                     'b3037ceafc1611ff662212a26623faa65c5921db',
                     '4999201b415fd669929a502e3550bea7e4a31b28',
                     '4fb451fc02f6adc46aff2193a5e41e922c759016',
                     '8bdabb108d5d5484248a29c06c066c51a68cbdbd',
                     '1a60655e471ac586d333353689342ff3fe6ea531',
                     '1539a1b39115f1421d9e5c0a0e4b339a174d33aa',
                     '57f8706fe1829947e25a22c2afe5cfa34e83e4c8',
                     'c602f2c0b1b2ab3aa7f637d0076233eccfce8ce5',
                     '0c3dc17f4caac1b4fa5f4d91453d8ece2850f19e'],
          'x-amz-bucket': 'drb_website_storage',
          'x-amz-key': 'devinberg.com/ASEE_AC2014-8679_final.pdf'},
 'info_hash': '64cba143eb0ef70ae025a66d74e8b83af30bb7f2',
 'url-list': ['https://s3.amazonaws.com/drb_website_storage/devinberg.com/ASEE_AC2014-8679_final.pdf']}

```

另外，我们也可以通过bencode-editor工具来查看metainfo文件。

![](/assets/img/posts/bittorrent/Snipaste_2019-03-24_20-23-10.png)

从以上的截图可以发现所有piece的SHA1哈希值总大小是320个字节，由于一个SHA1哈希值是由20个字节构成，所以共有16个piece。同时，从上图可以发现一个piece的大小是262144个bit。所以整个文件的大小大约是4M(16 * 262144 / 1024 /1024 = 4)，当然实际可能小于4M，因为最后一个piece可能要小于262144个bit。另外，将图中红色框框中的info部分进行hash，得到的值就是info_hash的值。

![](/assets/img/posts/bittorrent/Snipaste_2019-03-25_10-06-16.png)

bittorrent协议的节点发现需要借助tracker。节点与tracker通信可以通过HTTP协议或者UDP，当然节点与tracker之间通信所传输的数据量是非常小的。然而，如果使用HTTP协议，TCP链接的建立则会造成额外的开销，同时，使用HTTP每一次请求响应所传输的总数据量大约是1206个字节，而使用UDP，大约只需要618个字节。对于每一个客户端而言，这可能不太重要，但对于一个服务成千上万个客户端的tracker来说，减少50%的通信数据量所带来的影响将是巨大的。

理想情况下，使用UDP传输只需要两个包就可以了。但是由于由于可能会有虚假ip和UDP包，所以为了防止这样的事发生，所以tracker会计算出一个值（connection_id）并发送给客户端。如果客户端发送虚假ip和UDP包，那么tracker将不会收到这个值（除非客户端进行网络嗅探）。客户端在收到这个值之后的一分钟里都可以使用这个值，tracker应该在发送这个值之后的两分钟里接受这个值。更多关于连接过程的说明及示例，[官方文档](http://www.bittorrent.org/beps/bep_0015.html)里有详细解说，在此不再赘述。

为了更好的学习bittorrent协议，我fork了GitHub上的一个[bittorrent的实现](https://github.com/yoyoengineer/ttorrent)，并对其客户端进行了部分修改，使其可以通过HTTP接口启动和停止做种，同时也使得其可以同时对多个torrent进行做种。

## 系统架构

![](/assets/img/posts/bittorrent/structure.jpg)

修改后的系统由tracker服务器，用于做种的BitTorrent客户端，以及普通的BitTorrent客户端组成。至于管理员，则可以通过postman，curl等工具在任意一台机器上通过发送HTTP请求来控制用于做种的BitTorrent客户端。当然，tracker可以启动多个，之后只要在metainfo文件的announce-list里加上所有tracker的请求地址即可。tracker维护了各个BitTorrent客户端的相关信息，会根据客户端发送的请求中的info_hash值返回相应的peer list。接下来，各个客户端便会根据BitTorrent协议开始下载文件。

## 抓包分析

通过Wireshark对该系统的使用过程进行抓包，并对结果进行简单分析。

在使用过程中，主要是与云服务器进行通信，所以将过滤条件设为`ip.src == [云服务器ip] || ip.dst == [云服务器ip]`。由于该系统的Tracker是基于HTTP协议进行实现的。所以，首先从HTTP协议开始分析。

![1554451504643](/assets/img/posts/bittorrent/Snipaste_2019-04-04_11-26-36.png)

![](/assets/img/posts/bittorrent/Snipaste_2019-04-05_16-10-26.png)

从图中可以发现，先是postman向服务器发送了一条开始做种的请求，使Tracker开始了一个做种任务，其表单中携带有`torrentPath`，`metainfoPath`，`seed`，`domainName`等数据。

![](/assets/img/posts/bittorrent/Snipaste_2019-04-05_16-19-15.png)

![](/assets/img/posts/bittorrent/Snipaste_2019-04-05_19-56-20.png)

随后是本地BitTorrent客户端向Tracker服务器发起的请求，并携带有`info_hash`,`peer_id`等信息。而Tracker也返回了含有 "text/plain" MIME 类型文本数据的响应，其数据通过bencoding方式编码，并含有`complete`,`peers`等数据。此类HTTP请求，之后也有继续发送。关于请求响应中各项数据的具体含义解释，请参阅[文档](http://jonas.nitro.dk/bittorrent/bittorrent-rfc.html)。

![](/assets/img/posts/bittorrent/Snipaste_2019-04-05_20-16-00.png)

最后，是由postman发送的停止做种的请求，并携有hexInfoHash数据。

接下来是BitTorrent协议，输入`bittorrent`作为过滤条件。

![](/assets/img/posts/bittorrent/Snipaste_2019-04-05_20-24-45.png)

当然，由于BitTorrent协议内容较多，就不再一一分析说明，具体内容可对照文档分析查阅：

- [BitTorrent Protocol -- BTP/1.0](http://jonas.nitro.dk/bittorrent/bittorrent-rfc.html)
- [Bittorrent Protocol Specification v1.0](https://wiki.theory.org/index.php/BitTorrentSpecification)

## 系统使用过程

首先，为云服务器安装jdk。将本地jdk拷贝到云服务器。

```bash
scp -r [本地jdk所在目录] [远程jdk所要存放目录]
```

并在`/etc/profile`文件末尾添加环境变量相关配置。

```bash
vim /etc/profile
```

![](/assets/img/posts/bittorrent/Screenshot2019-03-29211622.jpg)

![](/assets/img/posts/bittorrent/Screenshot2019-03-29212546.jpg)

![](/assets/img/posts/bittorrent/Screenshot2019-03-29212728.jpg)

接下来，分别将tracker，client，以及制作metainfo文件的torrent-cli打成jar包。

![](/assets/img/posts/bittorrent/Screenshot2019-03-31201842.jpg)

![](/assets/img/posts/bittorrent/Screenshot2019-03-31201927.jpg)

![](/assets/img/posts/bittorrent/Screenshot2019-03-31204354.jpg)

![](/assets/img/posts/bittorrent/Screenshot2019-03-31203030.jpg)

打出来的jar包里的MANIFEST.MF里可能缺少`Main-Class`这个属性，因此，可以将jar包解压，在其中的MANIFEST.MF文件里加上`Main-Class`及对应值，并更新jar包。

```bash
jar umf MANIFEST.MF [要更新的jar包]
```

也可以使用其他方法修改MANIFEST.MF文件，并更新jar包，具体可参阅[官方文档](https://docs.oracle.com/javase/tutorial/deployment/jar/build.html)。

接下来，在云服务器上创建相应文件夹。在本地制作好metainfo文件，并将制作好的metainfo文件，torrent文件，相应jar包上传到云服务器上。

```
mkdir [directary name]
touch [metainfo file]
java -jar torrent-cli.jar -c -t [metainfo file] -a [announce address] [torrent file(s)]
scp ......
```

![](/assets/img/posts/bittorrent/Screenshot2019-04-02152206.jpg)

![](/assets/img/posts/bittorrent/Screenshot2019-04-02153438.jpg)

然后，我要启动云服务器上的tracker。

```
java -jar tracker-cli.jar
```

![](/assets/img/posts/bittorrent/Screenshot2019-04-02153928.jpg)

下一步是要启动云服务器上的bittorrent客户端让其进行做种。在此之前，需要先为云服务器的公网IP配一个域名，以便让客户端根据域名找到对应公网IP，并上报给tracker，让tracker记录其公网IP。

修改hosts文件，在其末尾添加IP及其对应域名。

```
vim /etc/hosts
```

![](/assets/img/posts/bittorrent/Screenshot2019-04-02154409.jpg)

![](/assets/img/posts/bittorrent/Screenshot2019-04-02164126.jpg)

随后可以启动bittorrent客户端，加上`-h`可以查看其用法。

```bash
java -jar client-cli.jar -h
java -jar client-cli.jar
```

![](/assets/img/posts/bittorrent/Screenshot2019-04-02165231.jpg)

![](/assets/img/posts/bittorrent/Screenshot2019-04-02165250.jpg)

接下来，可以在本地通过postman等工具发送HTTP请求，使其开始做种。当然，通过改变参数，发送多个请求，可以使其同时为多个torrent做种。

![](/assets/img/posts/bittorrent/Screenshot2019-04-02165514.jpg)

![](/assets/img/posts/bittorrent/Screenshot2019-04-02165802.jpg)

此时，可以在本地使用任意普通的Bittorrent客户端打开metainfo文件，开始下载文件。

```bash
java -jar normal_client-cli.jar -o /home/yu/torrent_downloads/peer0 /home/yu/test.torrent
java -jar normal_client-cli.jar -o /home/yu/torrent_downloads/peer1 /home/yu/test.torrent
```

![](/assets/img/posts/bittorrent/Screenshot2019-04-02165858.jpg)

![](/assets/img/posts/bittorrent/Screenshot2019-04-02171504.jpg)

如果在之前发送启动请求时设置的做种时间结束前，想要提前结束做种的话，只要发送一个停止的HTTP请求，并在请求参数中携带十六进制的infohash值。

![](/assets/img/posts/bittorrent/Screenshot2019-04-02171641.jpg)

![](/assets/img/posts/bittorrent/Screenshot2019-04-02171952.jpg)

最后，如果想要关掉云服务器上的tracker和client，只需要先使用如下命令，列出所有进程。再根据进程产生的命令（也就是之前启动时使用的命令），找到相关进程，并通过进程ID将对应进程杀掉就可以了。

```bash
ps -Af
kill [进程ID]
```

![](/assets/img/posts/bittorrent/Screenshot2019-04-02172115.jpg)

参考文献

[The BitTorrent Protocol Specification](http://www.bittorrent.org/beps/bep_0003.html)<br/>
[UDP Tracker Protocol for BitTorrent](http://www.bittorrent.org/beps/bep_0015.html)
