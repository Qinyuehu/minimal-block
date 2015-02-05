---
layout: post
title: "UBUNTU ENVIRONMENT PATH SETTINGS"
date: 2015-02-05 16:43:00
category: ubuntu
---
[原文](http://blog.sina.com.cn/s/blog_7cbaa68a0101fpbn.html)

Problem:
========

Sometimes wrong modification of environment path variable can cause system login
failure.
{% highlight bash %}
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games"
export JAVA_HOME=/opt/jdk1.7.0_07
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
{% enhighlight %}

由于最后红色标注的一行，导致系统启动到登录界面，输入密码后，一直报错，无法进入系统(正确设置jdk的方法请参考：http://www.linuxidc.com/Linux/2012-09/71209.htm)

SOLUTIONS:
==========

由于无法登录， 所以我们得从命令行下将前面我们错误修改的环境变量改正过来。
1. 在登录界面，按 Ctrl + Alt + F1 进入命令行模式。（Ctrl+Alt+F1-F6可以分别启动6个不同命令行， Ctrl+Alt+F7可以切换回UI界面）
2. 使用vim或者vi来更改环境变量，以我上面所述为例:`sudo vim /etc/environment`
由于环境变量的原因， 很多时候系统已经无法直接调用sudo 或者 vim 这样的命令，所以我们必须使用绝对路径：
`/usr/bin/sudo /usr/bin/vim /etc/environment`
3. 接下来就将环境变量修改为正确的，然后保存，退出，重启。

Ubuntu Environment path configration:
=====================================

Ubuntu Linux系统环境变量配置文件：
`/etc/profile` : 在登录时,操作系统定制用户环境时使用的第一个文件 ,此文件为系统的每个用户设置环境信息,当用户第一次登录时,该文件被执行。
 
`/etc /environment` : 在登录时操作系统使用的第二个文件, 系统在读取你自己的profile前,设置环境文件的环境变量。
 
`~/.profile` :  在登录时用到的第三个文件 是.profile文件,每个用户都可使用该文件输入专用于自己使用的shell信息,当用户登录时,该文件仅仅执行一次!默认情况下,他设置一些环境变量,执行用户的.bashrc文件。
 
`/etc/bashrc` : 为每一个运行bash shell的用户执行此文件.当bash shell被打开时,该文件被读取.

`~/.bashrc` : 该文件包含专用于你的bash shell的bash信息,当登录时以及每次打开新的shell时,该该文件被读取。
 
PASH环境变量的设置方法：

Solution I: ~/.profile or .bashrc
=================================
 
登录到你的用户（非root），在终端输入：
`$ sudo gedit ~/.profile(or .bashrc)`
可以在此文件末尾加入PATH的设置如下：
`export PATH=”$PATH:your path1:your path2 ...”`
保存文件，注销再登录，变量生效。
该方式添加的变量只对当前用户有效。 
`~/.bashrc `: 该文件包含专用于你的bash shell的bash信息,当登录时以及每次打开新的shell时,该该文件被读取。

Solution II: /etc/profile
=========================

在系统的etc目录下，有一个profile文件，编辑该文件：
`$ sudo gedit /etc/profile`
在最后加入PATH的设置如下：
`export PATH=”$PATH:your path1:your path2 ...”`
该文件编辑保存后，重启系统，变量生效。
该方式添加的变量对所有的用户都有效。

Solution III: /etc/environment
==============================

在系统的etc目录下，有一个environment文件，编辑该文件：
`$ sudo gedit /etc/environment`
找到以下的 PATH 变量：
`PATH="<......>"`
修改该 PATH 变量，在其中加入自己的path即可，例如：
`PATH="<......>:your path1:your path2 …"`
各个path之间用冒号分割。该文件也是重启生效，影响所有用户。 
注意这里不是添加export PATH=… 。

Solution IV: Directly in terminal
=================================

`$ sudo export PATH="$PATH:your path1:your path2 …"`
这种方式变量立即生效，但用户注销或系统重启后设置变成无效，适合临时变量的设置。

注 意：方法二和三的修改需要谨慎，尤其是通过root用户修改，如果修改错误，将可能导致一些严重的系统错误。因此笔者推荐使用第一种方法。另外嵌入式 Linux的开发最好不要在root下进行（除非你对Linux已经非常熟悉了！！），以免因为操作不当导致系统严重错误。

下面是一个对environment文件错误修改导致的问题以及解决方法示例：
 
问题：因为不小心在 etc/environment里设在环境变量导致无法登录
提示：不要在 etc/environment里设置 export PATH这样会导致重启后登录不了系统
解决方法：
在登录界面 alt +ctrl+f1进入命令模式，如果不是root用户需要键入（root用户就不许这么罗嗦，gedit编辑会不可显示）

`</usr/bin/sudo /usr/bin/vi /etc/environment>`
光标移到`export PATH**` 行，连续按 d两次删除该行；
输入:wq保存退出；
然后键入/sbin/reboot重启系统（可能会提示need to boot，此时直接power off）

个人最近使用较多的为`~/.profile`
<sudo gedit ~/.profile>
然后添加：`export PATH="$PATH:mtpath"`
重启系统即可
在没有环境变量启效时，完整路径运行程序。
查看环境变量：`echo $PATH`
