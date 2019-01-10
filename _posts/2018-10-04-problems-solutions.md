---
layout: post
title:  "Problems & Solutions"
date:   2018-10-04 13:25:00 +0800
featured-img: problems
categories: Problems
summary: These are some of the problems I encountered in my daily life and the corresponding solutions.
---

### Configure the Windows firewall to allow pings

If you have a firewall enabled in Windows, ping requests are blocked by default. This prevents the University Information Security Office (UISO) vulnerability scanners from functioning.

#### solution

To configure your firewall to allow pings, follow the appropriate instructions below.

1. Search for `Windows Firewall`, and click to open it.
2. Click Advanced Settings on the left.
3. From the left pane of the resulting window, click Inbound Rules.
4. In the right pane, find the rules titled File and Printer Sharing (Echo Request - ICMPv4-In).
5. Right-click each rule and choose Enable Rule.

![](/assets/img/posts/problems/Snipaste_2018-10-03_17-53-57.PNG)

![](/assets/img/posts/problems/Snipaste_2018-10-03_17-54-00.PNG)

![](/assets/img/posts/problems/Snipaste_2018-10-03_17-55-00.PNG)

### Revoke the access token to github

Sometimes, I will grant the access to my github to some applications like atom. But later, maybe I want to revoke the access.

#### solution

First, sign in the github. And then do as the following steps:

1. Settings
2. Applications
3. Authorized OAuth Apps
4. revoke the access

![](/assets/img/posts/problems/Snipaste_2018-10-04_16-05-57.png)

![](/assets/img/posts/problems/Snipaste_2018-10-04_16-24-13.png)

![](/assets/img/posts/problems/Snipaste_2018-10-04_16-16-39.png)



### api-ms-win-crt-runtime-l1-1-0.dll is missing

When I'm trying to use an application, I encountered this problem (*The program can’t start because api-ms-win-crt-runtime-l1-1-0.dll is missing from your computer*).

#### solution

Download [Visual C++ Redistributable for Visual Studio 2015](https://www.microsoft.com/en-in/download/details.aspx?id=48145) from Microsoft



### Cannot get the package of golang

when running `go get` command,  the problem shown in the following pictures often appear.

![](/assets/img/posts/problems/Snipaste_2019-01-10_18-38-15.png)

#### solution

![](/assets/img/posts/problems/Snipaste_2019-01-10_18-42-38.png)

