---
layout: post
title: "Python Web Deploy"
subtitle: ""
date: 2015-11-25 10:25:00
author: "rightpeter"
header-img: ""
---

[Referenced to nowamagic.net](http://www.nowamagic.net/academy/part/13/302)

# 安装Python (CentOS)

首先我们需要在服务器上安装一个比较新的 Python，CentOS 5.8 默认装的 Python 是 2.4.3。

    [root@nowamagic ~]# python -V
    Python 2.4.g

我们需要自己安装Python 2.7.5。但是值得注意的是，我们必须不能破坏系统的环境。因为几个关键的实用应用程序依赖于
Python 2.4.3。如果替换了系统的Python环境就会发生很多难以预见的错误，导致要重装系统。

## 下载和安装Python

有个一个非常重要的步骤是我们使用的是make altinstall。如果使用make install，你将会看到在系统中有两个不同版本的
Python在/usr/bin/目录中。这将会导致很多问题，而且不好处理。

    wget http://www.python.org/ftp/python/2.7.5/Python-2.7.5.tar.bz2
    tar jxvf Python-2.7.5.tar.bz2
    cd Python-2.7.5
    ./configure --prefix=/usr/local
    make && make altinstall

运行以上命令后，你可以在目录/usr/local/bin/python2.7 看到新编译的环境。

系统的环境 python 2.4.3 是在/usr/bin/python目录和 /usr/bin/python2.4 目录.

这里已经成功安装 2.7.5，如果没成功，可能需要安装开发工具盒一些额外的库。这些额外的库并不严格的需要，但是如果不
安装，新版本的python编译器可能没法工作。

    # yum groupinstall "Development tools"
    # yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel

# Python包管理工具之间的关系

当前的包管理工具链是 easy_install/pip + distribute/setuptools + distutils, 显得较为混乱。

而将来的工具链组合非常简单：pip + distutils2。

1. distutils : Python 自带的基本安装工具，适用于非常简单的应用场景，使用：
 * 为项目创建 setup.py 脚本。
 * 执行 setup.py install 可进行安装。

2. setuptools : 针对 distutils 做了大量扩展, 尤其是加入了包依赖机制. 在部分 Python 子社区已然是事实上的标准。

3. distribute : 由于 setuptools 开发进度缓慢，不支持 Python 3，代码混乱，一帮程序员另起炉灶，重构代码，增加功能，
希望能够取代 setuptools 并被接纳为官方标准库，他们非常努力，在很短的时间便让社区接受了 distribute。

4. easy_install : setuptools 和 distribute 自带的安装脚本，也就是一旦 setuptools 或 distribute 安装完毕，
easy_install 也便可用。最大的特点是自动查找 Python 官方维护的包源 PyPI，安装第三方 Python 包非常方便。使用：
 * setuptools / distribute 都只是扩展了 distutils。
 * easy_install [PACKAGE_NAME] 自动从 PyPI 查找/下载/安装指定的包。

5. pip : pip 的目标非常明确 – 取代 easy_install. easy_install 有很多不足：安装事务是非原子操作，只支持 svn，
没有提供卸载命令， 安装一系列包时需要写脚本。pip 解决了以上问题，已俨然成为新的事实标准，virtualenv 与它已经成
为一对好搭档。使用：
 * 安装: pip install [PACKAGE_NAME]
 * 卸载: pip uninstall [PACKAGE_NAME]
 * 支持从任意能够通过 VCS 或浏览器访问到的地址安装 Python 包

 6. distutils2 : setuptools 和 distribute 的诞生是因为 distutils 的不济，进而导致目前分化的状况。而 Guido 并未
 接纳 distribute 为官方标准，并说明了原因。开发者在失落之余明确了新的方向和任务 – distutils2，它将成为 Python
 3.3 的标准库 packaging，并在其它版本中以 distutils2 的身份出现。换句话说，它和 pip 将联手结束目前混乱的状况。

 7. zc.buildout : 这是一个臃肿的安装、部署系统，在 Zope 社区运用教广，功能强大/繁复但使用场景有限，除非确有需要，
 不值得投入太多的精力去研究，pip + virtualenv + fabric 的工具链组合更为简单、灵活。

# Python包管理工具Distribute的安装

Python的包管理工具常见的有easy_install, setuptools, 还有pip, distribute，那麽这几个工具有什么关系呢，看一下下面
这个图就明白了：

![Python Packages](http://www.nowamagic.net/librarys/images/201309/2013_09_02_01.png)

可以看到distribute是setuptools的替代方案，pip是easy_install的替代方案。

Distribute提供一个安装python模块的框架。你系统的每一个python解释器都需要它自己的Distribute。你可以自己找到最新
版本的Distribute，在这里https://pypi.python.org/pypi/distribute。

Distribute是对标准库disutils模块的增强，我们知道disutils主要是用来更加容易的打包和分发包，特别是对其他的包有依
赖的包。

Distribute被创建是因为Setuptools包不再维护了。

可以通过distribute_setup.py 脚本来安装Distribute，也可以通过easy_install, pip，源文件来安装，不过使用
distribute_setup.py来安装是最简单和受欢迎的方式。

    # wget https://pypi.python.org/packages/source/d/distribute/distribute-0.7.3.zip --no-check-certificate
    # unzip distribute-0.7.3.zip
    # cd distribute-0.7.3
    # python2.7 setup.py install

或者：

    # wget http://pypi.python.org/packages/source/d/distribute/distribute-0.6.35.tar.gz
    # tar xf distribute-0.6.35.tar.gz
    # cd distribute-0.6.35
    # python2.7 setup.py install

这将产生一个脚本/usr/local/bin/easy_install-2.7 ，你可以使用它来安装python 2.7 模块。它将安装的模块放到
/usr/local/lib/python2.7/site-packages/目录中

# 用Distribute安装PIP

前面我们花了很大篇幅去介绍包管理工具，并且安装了 Distribute。那么接下来我们就安装 pip。pip 是个包管理系统，使用
它能方便的安装我们想要的包。很简单，仅仅用一句话就能装上，因为我们装了 Distribute 了。

    [root@nowamagic ~]# easy_install pip

    Searching for pip
    Reading https://pypi.python.org/simple/pip/
    Best match: pip 1.4.1
    Downloading https://pypi.python.org/packages/source/p/pip/pip-1.4.1.tar.gz#md5=6afbb46aeb48abac658d4df742bff714
    Processing pip-1.4.1.tar.gz
    Writing /tmp/easy_install-Zv9xil/pip-1.4.1/setup.cfg
    Running pip-1.4.1/setup.py -q bdist_egg --dist-dir /tmp/easy_install-Zv9xil/pip-1.4.1/egg-dist-tmp-oAg3wu
    warning: no files found matching '*.html' under directory 'docs'
    warning: no previously-included files matching '*.rst' found under directory 'docs/_build'
    no previously-included directories found matching 'docs/_build/_sources'
    Adding pip 1.4.1 to easy-install.pth file
    Installing pip script to /usr/local/bin
    Installing pip-2.7 script to /usr/local/bin

    Installed /usr/local/lib/python2.7/site-packages/pip-1.4.1-py2.7.egg
    Processing dependencies for pip
    Finished processing dependencies for pip

    [root@nowamagic ~]# pip

有了 pip，部署 Python 环境就简单许多了。

# 用virtualenv建立多个Python独立开发环境

不同的人喜欢用不同的方式建立各自的开发环境，但在几乎所有的编程社区，总有一个（或一个以上）开发环境让人更容易接
受。 使用不同的开发环境虽然没有什么错误，但有些环境设置更容易进行便利的测试，并做一些重复/模板化的任务，使得在
每天的日常工作简单并易于维护。

## 什么是virtualenv？

在Python的开发环境的最常用的方法是使用 virtualenv 包。 Virtualenv是一个用来创建独立的Python环境的包。现在，出现
了这样的问题：为什么我们需要一个独立的Python环境？ 要回答这个问题，请允许我引用virtualenv自己的文档：

>virtualenv is a tool to create isolated Python environments.
>
>The basic problem being addressed is one of dependencies and versions, and indirectly permissions. Imagine you
>have an application that needs version 1 of LibFoo, but another application requires version 2. How can you use
>both these applications? If you install everything into /usr/lib/python2.7/site-packages (or whatever your
>platform’s standard location is), it’s easy to end up in a situation where you unintentionally upgrade an
>application that shouldn’t be upgraded.
>
>Or more generally, what if you want to install an application and leave it be? If an application works, any
>change in its libraries or the versions of those libraries can break the application.
>
>Also, what if you can’t install packages into the global site-packages directory? For instance, on a shared
>host.
>
>In all these cases, virtualenv can help you. It creates an environment that has its own installation directories,
>that doesn’t share libraries with other virtualenv environments (and optionally doesn’t access the globally
>installed libraries either).
>
>我们需要处理的基本问题是包的依赖、版本和间接权限问题。想象一下，你有两个应用，一个应用需要libfoo的版本1，而另一
>应用需要版本2。如何才能同时使用这些应用程序？如果您安装到的/usr/lib/python2.7/site-packages（或任何平台的标准位
>置）的一切，在这种情况下，您可能会不小心升级不应该升级的应用程序。

简单地说，你可以为每个项目建立不同的/独立的Python环境，你将为每个项目安装所有需要的软件包到它们各自独立的环境中。

# 安装与使用virtualenv

安装 virtualenv 很简单：

    pip install virtualenv

virtualenv安装完毕后，可以通过运行下面的命令来为你的项目创建独立的python环境：

    mkdir nowamagic_venv
    virtualenv --distribute nowamagic_venv

OK，成功。上面发生了什么？你创建了文件夹 nowamagic_venv 来存储你的新的独立Python环境。 这个文件夹位于 /root 下
面。

我们再来看看输出：

    New python executable in nowamagic_venv/bin/python2.7
    Also creating executable in nowamagic_venv/bin/python
    Installing Setuptools......done.
    Installing Pip...........done.

`--distribute`选项使virtualenv使用新的基于发行版的包管理系统而不是setuptools获得的包。你现在需要知道的就是
`--distribute`选项会自动在新的虚拟环境中安装pip，这样就不需要手动安装了。当你成为一个更有经验的Python开发者，
你就会明白其中细节。

![Virtualenv](http://www.nowamagic.net/librarys/images/201309/2013_09_03_01.png)

 * activate：这个virtualenv的激活文件
 * pip：这个virtualenv的独立pip
 * python：python解释器的一个副本
 * lib/python2.7：所有的新包会被存在这

## 试验一下

通过下面的命令激活这个virtualenv：

    [root@nowamagic ~]# cd nowamagic_venv
    [root@nowamagic nowamagic_venv]# source bin/activate
    (nowamagic_venv)[root@nowamagic nowamagic_venv]#

运行下面的命令可以更好地理解两者的差异，如果已经进入virtualenv请先离开：

    deactivate  #离开

首先让我们看看如果调用python/pip命令它会调用那一个：

    [root@nowamagic ~]# which python
    /usr/bin/python

    [root@nowamagic ~]# which pip
    /usr/local/bin/pip

再来一次！这次打开virtualenv，然后看看有什么不同。我的机子上显示如下：

    [root@nowamagic ~]# which python
    /root/nowamagic_venv/bin/python

    [root@nowamagic ~]# which pip
    /root/nowamagic_venv/bin/pip

virtualenv拷贝了Python可执行文件的副本，并创建一些有用的脚本和安装了项目需要的软件包，你可以在项目的整个生命周
期中安装/升级/删除这些包。 它也修改了一些搜索路径，例如PYTHONPATH，以确保：

 * 当安装包时，它们被安装在当前活动的virtualenv里，而不是系统范围内的Python路径。
 * 当import代码时，virtualenv将优先采取本环境中安装的包，而不是系统Python目录中安装的包。

还有一点比较重要，在默认情况下，所有安装在系统范围内的包对于virtualenv是可见的。 这意味着如果你将simplejson安装
在您的系统Python目录中，它会自动提供给所有的virtualenvs使用。 这种行为可以被更改，在创建virtualenv时增加
`--no-site-packages`选项的virtualenv就不会读取系统包，如下：

    virtualenv nowamagic_venv --no-site-packages

# Virtualenvwrapper多环境管理扩展

不厌其烦地再介绍下背景。

一般而言，所有python相关的包会装在系统目录里，譬如/usr/lib/ 或者/usr/local/lib/，这样的话，假设两个开发分支要求
的库不一样，譬如对应在线版本的开发环境使用 Django1.3，但是一个新的开发分支基于Django1.4，两者就会互相影响。

Virtualenv 是一个虚拟环境程序，可以把开发环境隔离。基本思想是建立不同的环境目录，其中装有独立的各类包，甚至也可
以是独立的不同版本python程序。

Virtualenv 和 virtualenvwrapper，前者已经介绍，后者是一套很有用的扩展，提供了方便切换开发环境的快捷命令。

Virtualenvwrapper 是一个建立在 virtualenv 上的工具，通过它可以方便的创建/激活/管理/销毁虚拟环境，没它的话进行上
面的操作将会相当麻烦。 可以通过下面命令安装 virtualenvwrapper 。

    pip install virtualenvwrapper

安装后，你需要配置它。下面是我的配置：

	if [ `id -u` != '0' ]; then

	export VIRTUALENV_USE_DISTRIBUTE=1        # <-- Always use pip/distribute
	export WORKON_HOME=$HOME/.virtualenvs       # <-- Where all virtualenvs will be stored
	source /usr/local/bin/virtualenvwrapper.sh
	export PIP_VIRTUALENV_BASE=$WORKON_HOME
	export PIP_RESPECT_VIRTUALENV=true

	fi

设置 WORKON_HOME 和 source /usr/local/bin/virtualenvwrapper.sh 只需要几行代码，别的部分是按照我个人喜好添加的。

将上面的配置添加到 ~/.bashrc 的末尾，然后将下面的命令运行一次：

	source ~/.bashrc

如果你关闭所有的shell窗口和标签，然后再打开一个新的shell窗口或标签时， ~/.bashrc 也会被执行，此时将会自动的更新
你的 virtualenvwrapper 配置。效果就跟执行上面的命令一样。

新建/激活/关闭/删除虚拟空间需要执行下面的命令：

	mkvirtualenv nowamagic_venv
	workon nowamagic_venv
	deactivate
	rmvirtualenv nowamagic_venv

Tab补全在virtualenvwrapper中是可用的。更多信息可以前往 virtualenvwrapper 的首页：[Virtualenvwrapper](http://www.doughellmann.com/projects/virtualenvwrapper/)

# 使用RPM安装Nginx

对于非 SA 的 Linux 使用者来说，不是很建议任何形式的自己编译安装，只用现成的包管理，不会出问题的。所以安装 Nginx，
以 CentOS 5.8 为例，这里采用 RPM 方式安装 Nginx。

CentOS 5.8 执行下面命令：

	wget http://nginx.org/packages/centos/5/noarch/RPMS/nginx-release-centos-5-0.el5.ngx.noarch.rpm
	rpm -ivh nginx-release-centos-5-0.el5.ngx.noarch.rpm
	yum install nginx

检查 Nginx 是否安装成功：

	[root@nowamagic ~]# whereis nginx
	nginx: /usr/sbin/nginx /etc/nginx /usr/share/nginx

查看 Nginx 版本：

	[root@nowamagic ~]# /usr/sbin/nginx -v
	nginx version: nginx/1.4.2

如果是 CentOS 6 以上，RPM 地址为
http://nginx.org/packages/centos/6/noarch/RPMS/nginx-release-centos-6-0.el6.ngx.noarch.rpm。

如果过程不顺利，请检查下 yum 是否过旧：

	yum -y update

安装依赖包：

	yum install gcc pcre pcre-devel zlib zlib-devel openssl openssl-devel

等等。

下面是编译安装 Nginx：

	wget http://nginx.org/download/nginx-1.4.2.tar.gz  
	tar -zxvf nginx-1.4.2.tar.gz
	mv nginx-1.4.2 nginx
	cd nginx
	./configure --prefix=/usr/local/nginx # 编译选项配置，这里从简
	make && make install # 编译安装
	ln -s /usr/local/nginx/sbin/nginx /usr/sbin/nginx

# 简单总结一些Nginx常用命令

Nginx 安装之后会成为我们服务器管理日常的一个重要部分，所以我们需要知道一些常见的 Nginx 操作命令。

1. 启动 Nginx


	/usr/sbin/nginx
	or
	nginx

路径可以通过 # whereis nginx 命令看。

2. 停止 Nginx


	nginx -s stop

3. 查看 Nginx 进程


	ps -ef|grep nginx

4. 平滑启动nginx


	nginx -s reload
	or
	kill -HUP `cat /var/run/nginx.pid`

其中进程文件路径在配置文件nginx.conf中可以找到。

平滑启动的意思是在不停止nginx的情况下，重启nginx，重新加载配置文件，启动新的工作线程，完美停止旧的工作线程。

5. 强制停止nginx


	pkill -9 nginx

6. 检查对nginx.conf文件的修改是否正确


	nginx -t -c /etc/nginx/nginx.conf
	or
	nginx -g
7. 查看nginx的版本信息


	nginx -v
	or
	nginx -V

# 列举一些常见的Python HTTP服务器

......

# <a name="WSGI"></a>来了解一下WSGI这个概念

## WSGI是什么？

WSGI，全称 Web Server Gateway Interface，或者 Python Web Server Gateway Interface ，是为 Python 语言定义的 Web
服务器和 Web 应用程序或框架之间的一种简单而通用的接口。自从 WSGI 被开发出来以后，许多其它语言中也出现了类似接口。

WSGI 的官方定义是，the Python Web Server Gateway Interface。从名字就可以看出来，这东西是一个Gateway，也就是网关。
网关的作用就是在协议之间进行转换。

WSGI 是作为 Web 服务器与 Web 应用程序或应用框架之间的一种低级别的接口，以提升可移植 Web 应用开发的共同点。WSGI
是基于现存的 CGI 标准而设计的。

很多框架都自带了 WSGI server ，比如 Flask，webpy，Django、CherryPy等等。当然性能都不好，自带的 web server 更多
的是测试用途，发布时则使用生产环境的 WSGI server或者是联合 nginx 做 uwsgi 。

>也就是说，WSGI就像是一座桥梁，一边连着web服务器，另一边连着用户的应用。但是呢，这个桥的功能很弱，有时候还需要
>别的桥来帮忙才能进行处理。WSGI 的作用如图所示：
![WSGI](http://www.nowamagic.net/librarys/images/201309/2013_09_04_01.png)

## WSGI的作用

WSGI有两方：“服务器”或“网关”一方，以及“应用程序”或“应用框架”一方。服务方调用应用方，提供环境信息，以及一个回调
函数（提供给应用程序用来将消息头传递给服务器方），并接收Web内容作为返回值。

所谓的 WSGI中间件同时实现了API的两方，因此可以在WSGI服务和WSGI应用之间起调解作用：从WSGI服务器的角度来说，中间
件扮演应用程序，而从应用程序的角度来说，中间件扮演服务器。“中间件”组件可以执行以下功能：
 * 重写环境变量后，根据目标URL，将请求消息路由到不同的应用对象。
 * 允许在一个进程中同时运行多个应用程序或应用框架。
 * 负载均衡和远程处理，通过在网络上转发请求和响应消息。
 * 进行内容后处理，例如应用XSLT样式表。

WSGI 的设计确实参考了 Java 的 servlet。http://www.python.org/dev/peps/pep-0333/ 有这么一段话：
>By contrast, although Java has just as many web application frameworks available, Java's "servlet" API makes
>it possible for applications written with any Java web application framework to run in any web server that
>supports the servlet API.

# HTTP 1.1的详细介绍

那么 HTTP 1.1 究竟是怎么一回事呢？我们选择 HTTP 服务器在什么情况下需要考虑对 HTTP 1.1 的支持？

## HTTP 1.1与HTTP 1.0的比较

一个WEB站点每天可能要接收到上百万的用户请求，为了提高系统的效率，HTTP 1.0规定浏览器与服务器只保持短暂的连接，浏
览器的每次请求都需要与服务器建立一个TCP连接，服务器完成请求处理后立即断开TCP连接，服务器不跟踪每个客户也不记录
过去的请求。

但是，这也造成了一些性能上的缺陷，例如，一个包含有许多图像的网页文件中并没有包含真正的图像数据内容，而只是指明
了这些图像的URL地址，当WEB浏览器访问这个网页文件时，浏览器首先要发出针对该网页文件的请求，当浏览器解析WEB服务器
返回的该网页文档中的HTML内容时，发现其中的<img>图像标签后，浏览器将根据<img>标签中的src属性所指定的URL地址再次
向服务器发出下载图像数据的请求，如下图所示。

![HTTP1.1](http://www.nowamagic.net/librarys/images/201309/2013_09_03_03.jpg)

显然，访问一个包含有许多图像的网页文件的整个过程包含了多次请求和响应，每次请求和响应都需要建立一个单独的连接，
每次连接只是传输一个文档和图像，上一次和下一次请求完全分离。即使图像文件都很小，但是客户端和服务器端每次建立和
关闭连接却是一个相对比较费时的过程，并且会严重影响客户机和服务器的性能。当一个网页文件中包含Applet，JavaScript
文件，CSS文件等内容时，也会出现类似上述的情况。

为了克服HTTP 1.0的这个缺陷，HTTP 1.1支持持久连接，在一个TCP连接上可以传送多个HTTP请求和响应，减少了建立和关闭连
接的消耗和延迟。一个包含有许多图像的网页文件的多个请求和应答可以在一个连接中传输，但每个单独的网页文件的请求和
应答仍然需要使用各自的连接。HTTP 1.1还允许客户端不用等待上一次请求结果返回，就可以发出下一次请求，但服务器端必须按照接收到客户端请求的先后顺序依次回送响应结果，以保证客户端能够区分出每次请求的响应内容，这样也显著地减少了整个下载过程所需要的时间。基于HTTP 1.1协议的客户机与服务器的信息交换过程，如下图所示。

![HTTP1.1](http://www.nowamagic.net/librarys/images/201309/2013_09_03_04.jpg)

可见，HTTP 1.1在继承了HTTP 1.0优点的基础上，也克服了HTTP 1.0的性能问题。不仅如此，HTTP 1.1还通过增加更多的请求
头和响应头来改进和扩充HTTP 1.0的功能。例如，由于HTTP 1.0不支持Host请求头字段，WEB浏览器无法使用主机头名来明确表
示要访问服务器上的哪个WEB站点，这样就无法使用WEB服务器在同一个IP地址和端口号上配置多个虚拟WEB站点。

在HTTP 1.1中增加Host请求头字段后，WEB浏览器可以使用主机头名来明确表示要访问服务器上的哪个WEB站点，这才实现了在
一台WEB服务器上可以在同一个IP地址和端口号上使用不同的主机名来创建多个虚拟WEB站点。HTTP 1.1的持续连接，也需要增
加新的请求头来帮助实现，例如，Connection请求头的值为Keep-Alive时，客户端通知服务器返回本次请求结果后保持连接；
Connection请求头的值为close时，客户端通知服务器返回本次请求结果后关闭连接。HTTP 1.1还提供了与身份认证、状态管理
和Cache缓存等机制相关的请求头和响应头。

>HTTP 协议老的标准是HTTP/1.0，目前最通用的标准是HTTP/1.1。HTTP/1.1是在HTTP/1.0基础上的升级，增加了一些功能，全
>面兼容 HTTP/1.0。HTTP/1.0不支持文件断点续传，目前的Web服务器绝大多数都采用了HTTP/1.1。

PS：RANGE:bytes是HTTP/1.1新增内容，HTTP/1.0每次传送文件都是从文件头开始，即0字节处开始。RANGE:bytes=XXXX表示要
求服务器从文件XXXX字节处开始传送，这就是我们平时所说的断点续传。

# uWSGI其一：概念篇

## 复习 WSGI

前面小节[《来了解一下WSGI这个概念》](#WSGI)已经详细介绍过 WSGI 了。

>WSGI is the Web Server Gateway Interface. It is a specification for web servers and application servers to
>communicate with web applications (though it can also be used for more than that)

WSGI是一种Web服务器网关接口。它是一个Web服务器（如nginx）与应用服务器（如uWSGI服务器）通信的一种规范。

接下来，我们要介绍的是 uWSGI。

## uWSGI

uWSGI是一个Web服务器，它实现了WSGI协议、uwsgi、http等协议。Nginx中HttpUwsgiModule的作用是与uWSGI服务器进行交换。

要注意 WSGI / uwsgi / uWSGI 这三个概念的区分。

 * WSGI看过前面小节的同学很清楚了，是一种通信协议。
 * uwsgi同WSGI一样是一种通信协议。
 * 而uWSGI是实现了uwsgi和WSGI两种协议的Web服务器。

uwsgi协议是一个uWSGI服务器自有的协议，它用于定义传输信息的类型（type of information），每一个uwsgi packet前
4byte为传输信息类型描述，它与WSGI相比是两样东西。

关于uwsgi协议看这里：[The uwsgi protocol](http://projects.unbit.it/uwsgi/wiki/uwsgiProtocol)。

>为什么有了uWSGI为什么还需要nginx？因为nginx具备优秀的静态内容处理能力，然后将动态内容转发给uWSGI服务器，这样可
>以达到很好的客户端响应。
