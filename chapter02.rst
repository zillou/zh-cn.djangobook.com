==========================
第二章: 入门
==========================

由于现代Web开发环境由多个部件组成，安装Django需要几个步骤。这一章，我们将演示如何安装Django和
他的一些依赖。

因为Django就是Python代码，所以他可以运行在任何Python可以运行的环境，甚至是手机上！但是本章只
介绍安装在桌面/笔记本或服务器上安装Django这种最普遍的情况，

稍后，我们会在 第12章_ 介绍如何部署Django站点。

安装Python
=================

Django本身是由Python编写的，所以安装Django的第一步是确保你已经安装了Python。

Python的版本选择
------------------

Django的核心(1.4+)可以运行在从2.5到2.7之间的任何Python版本。Django的可选GIS(地理信息系统)
支持则需要Python 2.5到2.7的版本。

如果你不确定要安装哪个Python版本，并且你也可以随意选择的话，我们建议选择2.x系列中的最新版本：
2.7。虽然Django在2.5到2.7的版本之间都一样运行良好，但是新版本的Python会有一些性能提升和新增的语言特性，你可以将其用到你
的程序中。另外，一些你可能会用到Django的三方插件可能会要求比2.5更新的Python版本，所以选择较新
的Python版本可以让你由更多选择。


.. admonition:: Django和Python 3.x

    在写作本书的时候，Python3.3已经发布，但是Django对Python3的支持还只是实验性的(django1.5.x)。
    因为Python3.x引入了相当多的不向后兼容的更新，目前很多主要的Python类库和框架(包括
    Python1.4)都还没能跟上。

    Django 1.5预计会支持Python2.6，2.7和3.2。不过这只是一种预览性质的支持，Django的开发者
    们还没有足够的信心来保证生产环境的稳定性。如果你想要用在Python3中使用Django，他们建议等到
    Django 1.6发布。


安装
------------

如果你是用Linux或者Mac OS X，你的系统很可能已经预装了Python。在命令提示符(OS X中的
Applications/Utilities/Terminal)中输入 ``python`` ， 如果你看到类似于下面的信息，
就说明Python已经安装好了。

::

    Python 2.7.3rc2 (default, Apr 22 2012, 22:30:17)
    [GCC 4.6.3] on linux2
    Type "help", "copyright", "credits" or "license" for more information.
    >>>

如果没有的话，你需要去下载并安装Python。这个步骤很快也很容易，详细的说明在 http://www.python.org/download/

安装Django
=================

任何时候，都有两个不同版本的Django可供您选择：最新的正式发行版和最前沿的开发版本。安装哪个版本
取决于你。你是想要一个稳定的经过测试的Django还是一个包括最新功能，但是可能不那么稳定的Django
(也许你可以对Django本身做贡献)。

我们推荐正式发行版，但是知道有开发版的存在也很重要，因为在文档和社区中常常有人提到它。

安装正式发行版
------------------------------

正式发行版都有一个版本号，例如1.4.2，1.4.1，或者1.4。最新的版本可以在 http://www.djangoproject.com/download/ 找到。

如果你使用的Linux发行版(译者：比如Ubuntu)包括了Django的包，用包管理器安装也很好，这样你可以
通过系统的包管理的到安全的升级。

如果你找不到一个已经打包好的Django，你可以自己下载安装。首先，下载安装包，名字类似于
``Django-1.4.2.tar.gz`` 这种。(下载到哪里都行，安装程序会自动的把Django安装到正确的位置。)
然后，和大多数Python的类库一样，解压这个压缩包，运行 ``setup.py install`` 就可以了。

以下是在Unix系统中安装的方法:

#. ``tar xzvf Django-1.4.2.tar.gz``
#. ``cd Django-*``
#. ``sudo python setup.py install``

在Windows中，我们推荐用7-Zip (http://www.djangoproject.com/r/7zip/)来解压 ``.tar.gz`` 文件。
解压完这个文件后，以系统管理员权限打开一个DOS窗口(命令提示符 cmd)窗口，在刚刚解压出来的以
``Django-`` 开头的目录中运行下面这个命令，

::

    python setup.py install

如果你好奇的话，Django会被安装到Python安装目录中的 ``site-packages`` 目录中(Python从该目录中
寻找三方库)。通常情况下，这个目录的绝对路径是
``/usr/lib/python2.7/site-packages``.

Installing the "Development" Version
------------------------------------

Django uses Git (http://git-scm.com) for its source control. The latest and
greatest Django development version available from Django's official Git
repository (https://github.com/django/django). You should consider installing
this version if you want to work on the bleeding edge, or if you want to
contribute code to Django itself.

Git is a free, open source distributed revision-control system, and the Django
team uses it to manage changes to the Django codebase. You can download and
install Git from http://git-scm.com/download but it is easier to install with
your operating system's package manager. You can use Git to grab the very latest
Django source code and, at any given time, you can update your local version of
the Django code to get the latest changes and improvements made by Django
developers.

When using the development version, keep in mind there's no guarantee things
won't be broken at any given moment. With that said, though, some members of the
Django team run production sites on the development version, so they have an
incentive to keep it stable.

To grab the latest Django, follow these steps:

#. Make sure you have Git installed. You can get the
   software free from http://git-scm.com/, and you can find
   excellent documentation at http://git-scm.com/documentation.

#. Clone the repository using the command ``git clone https://github.com/django/django djmaster``

#. Locate your Python installation's ``site-packages`` directory. Usually
   it's in a place like ``/usr/lib/python2.7/site-packages``. If you have
   no idea, type this command from a command prompt::

       python -c 'import sys, pprint; pprint.pprint(sys.path)'

   The resulting output should include your ``site-packages`` directory.

#. Within the ``site-packages`` directory, create a file called
   ``djmaster.pth`` and edit it to contain the full path to your ``djmaster``
   directory to it. For example, the file could just contain this line::

       /path/to/djmaster

#. Place ``djmaster/django/bin`` on your system PATH. This directory
   includes management utilities such as ``django-admin.py``.

.. admonition:: Tip:

    If ``.pth`` files are new to you, you can learn more about them at
    http://www.djangoproject.com/r/python/site-module/.

After downloading from Git and following the preceding steps, there's no
need to run ``python setup.py install``-- you've just done the work by hand!

Because the Django code changes often with bug fixes and feature additions,
you'll probably want to update it every once in a while. To update the code,
just run the command ``git pull origin master`` from within the ``djmaster``
directory. When you run that command, Git will contact
https://github.com/django/django, determine whether any of Django's code has
changed, and update your local version of the code with any changes that have
been made since you last updated. It's quite slick.

Finally, if you use Django development version, you should know how to figure
out which version of Django you're running. Knowing your version number is
important if you ever need to reach out to the community for help, or if you
submit improvements to the framework. In these cases, you should tell people the
revision, also known as a "commit," that you're using. To find out your current
commit, type "git log -1" from within the ``django`` directory, and look for the
identifier after "commit". This number changes each time Django is changed,
whether through a bug fix, feature addition, documentation improvement or
anything else.

检查Django的安装
===============================

让我们花点时间检查一下Django是否已经安装成功，并工作良好。在一个命令提示符窗口中，切换到另外一个目录
（确保不是包含Django的目录），然后输入 ``python`` 来打开Python的交互解释器(interactive interpreter)
。如果安装是成功的，你现在应该可以导入 ``django`` 模块了：

::

    >>> import django
    >>> django.VERSION
    (1, 4, 2, 'final', 0)

.. admonition:: 交互解释器示例

    Python的交互解释器是一个命令行窗口程序，它可以让你交互式的编写Python程序。要启动它只需要运行 ``python``
    命令。

    整本书里，我们都会在Python交互解释器中演示Python示例。示例的前面都有三个大于号( ``>>>`` )，三个大于号
    就表示交互提示符。如果你要从书中拷贝示例代码，请不要包括提示符。

    在交互式解释器中，多行声明用三个点 (``...``)来填补，例如：

    ::

        >>> print """This is a
        ... string that spans
        ... three lines."""
        This is a
        string that spans
        three lines.
        >>> def my_function(value):
        ...     print value
        >>> my_function('hello')
        hello

    这三个在新行开始插入的点，是由Python Shell自动添加的，不需要我们输入。但是我们为了更接近
    真实的输出，我们保留了这三个点。同样，你要拷贝代码去运行的话，不要包括这些点。

安装数据库
=====================

Django安装好了之后，你就可以使用Django来编写Web程序了，因为Django只要求一个Python运行环境。
不过，大多数情况下，你是要开发一个 *数据库驱动* 的站点，这时你需要去配置一个数据库服务器。

如果你只是像体验以下Django，你可以直接跳到“创建一个项目(project)”部分去。不过本书的例子都是基于你
已经配置号了一个正常工作的数据库。

Django支持四种数据库:

* PostgreSQL (http://www.postgresql.org/)
* SQLite 3 (http://www.sqlite.org/)
* MySQL (http://www.mysql.com/)
* Oracle (http://www.oracle.com/)

大部分情况下，这四种数据库都可以在Django中很好的运行。（值得注意的是Django的GIS支持运行在PostgreSQL下要明显
好于其他数据库）如果你不准备在一个很老旧的系统上运行，并且你可以自由选择所有的数据库的话，我们推荐PostgreSQL。
它在成本，特性，速度和稳定性方面都做得比较平衡。

设置数据库只需要两步：

* 首先，你需要安装和配置数据库本身。这个过程已经超出了本书的内容。好在你在这四种数据库的网站上都可以找到丰富的文档。（如果你用的是共享主机，有可能他们已经为你安装好了。）

* 然后，你需要为你的数据库后端安装必要的Python库。这是一些允许Python连接数据库的第三方代码。马上，我们会为每种数据库单独列出需要安装的东西。

如果你不想安装数据库，只是想尝试以下Django，考虑使用SQLite。SQLite在这四种数据库中很特别，它不需要上面的任何步骤。
它仅对你文件系统中的一个文件进行读写，Python2.5之后的版本都内建了对SQLite的支持。  

在Windows上，安装数据库驱动是件很烦人的事。如果你着急要体验Django，我们建议是用Python 2.7和它内建支持的SQLite。

在Django使用PostgreSQL
----------------------------

如果你要使用PostgreSQL的话，你需要从 http://www.djangoproject.com/r/python-pgsql/ 下载
``psycopg`` 或者 ``psycopg2`` 包。我们推荐使用 ``psycopg2`` ,因为它更新一些，开发也活跃一些，也更
容易安装。安装哪个都行，只是要注意记着你用的到底是那个版本，1还是2，后面会用到。

如果你是在Windows上使用PostgreSQL，你可以到 http://www.djangoproject.com/r/python-pgsql/windows/ 下载
预编译好的 ``psycopg``  。

如果用Linux的话，查看你的发行版是否提供了“python-psycopg2”, “psycopg2-python”或类似名字的包。

在Django使用SQLite 3
--------------------------

如果你用SQLite 3的话，你很幸运，你不需要去安装一个特定的数据库了，因为Python自带SQLite支持。直接跳到下一节吧。

在Django使用MySQL
-----------------------

Django要求MySQL4.0或更新的版本。3.x版本不支持嵌套子查询和其他一些相当标准的SQL语句。

你还需要从 http://www.djangoproject.com/r/python-mysql/ 下载安装 ``MySQLdb`` 包。

如果你使用Linux，查看一下你的包管理器是否提供了叫做“python-mysql”, “python-mysqldb”, “mysql-python” 或者
类似名字的包。

在Django使用Oracle
------------------------

Django要求Oracle9i或者更高的版本。

如果你使用Oracle， 你需要安装 ``cx_Oracle`` 库，可以在 http://cx-oracle.sourceforge.net/ 获得。
请使用4.3.1或更高的版本，但是要避免使用5.0这个版本，这个版本的驱动有Bug。5.0.1版本就修复了这个bug了，你可以使用
它以上的版本。

不使用数据库
-------------------------------

前面有提到，Django并不要求一定要有数据库。如果你只是需要提供一些不设计数据库的动态页面的话，Django也
是美欧问题的。

尽管这样，还是要注意，Django捆绑的一些工具是必须要求数据库的。如果你不用数据库，你就不能使用那些功能了。
（这些功能是本书的重点）


创建一个项目(project)
=====================

安装好Python，Django，配置好数据库(这一步不是必须的)之后。你就可以迈出器开发的第一步： 创建一个 *project* 。

Django的project是一个Django的示例，包括一系列的设置，如数据库的设置，Django特定的选项以及你的程序的一些配置。

如果第一次使用Django，你需要做一些初始化工作。新建一个工作目录，比如 ``/home/username/djcode/`` 。

.. admonition:: 这个目录应该放在哪儿？

    如果你有过PHP背景的话，你可能习惯于把代码放到Web服务器的根目录(比如 ``/var/www`` )。 但是在Django中
    不要这样做，这样你的源代码有可能被人通过网络查看到，这可不好。
    
    把代码放在文档根目录 **之外** 的目录中。

进到你刚刚创建的目录，运行 ``django-admin.py startproject mysite`` 。 这条命令会在当前目录下创建一个
``mysite`` 子目录。

.. note::

    如果你是用 ``setup.py`` 安装的Django， ``django-admin.py`` 应该已经在你系统的 ``PATH`` 环境变量中了。

    如果你用的是开发版，``django-admin.py`` 在 ``djmaster/django/bin`` 目录下。因为我们会经常用到 ``django-admin.py``
    建议把它加到系统的 ``PATH`` 环境变量中。Unix里，你可以通过命令 ``sudo ln -s
    /path/to/django/bin/django-admin.py /usr/local/bin/django-admin.py``
    在 ``/usr/local/bin`` 创建一个符号连接。 Windows中你需要修改你的 ``PATH`` 环境变量。

    如果你直接通过Linux发行版自带的包安装的话，``django-admin.py`` 可能被改成了 ``django-admin``， 运行
    ``django-admin`` 就可以了。

如果你运行 ``django-admin.py startproject`` 碰到“permission denied”这样的错误。你需要修改这个文件的权限。
进入到存放 ``django-admin.py`` 的目录(如 ``cd /usr/local/bin``)，运行 ``chmod +x django-admin.py`` 。

``startproject`` 命令会创建一个文件，里面包括5个文件：

::

    mysite/
        manage.py
        mysite/
            __init__.py
            settings.py
            urls.py
            wsgi.py

.. note:: 你的不一样吗？
    
    项目默认的结构在最近的版本中有所改变。如果你的项目中不再有里面那个 ``mysite`` 目录，可能你使用的
    Django版本和本书使用的版本不一致。本书适用的是Djang 1.4或者更高的版本，所以，如果你使用的是更老的
    版本，请查阅Django的官方文档
    
    Django 1.X的文档在 https://docs.djangoproject.com/en/1.X/ 。

文件如下:

* ``mysite/``: 外面这个 ``mysite/`` 目录只是你包含你的项目的目录，
  Django并不在意它的名字，你可以将它重命名成你任何你喜欢的名字。 

* ``manage.py``: 命令行工具集，供你去操作本Django项目。输入 ``python manage.py help`` 可以查看
  它到底提供了哪些功能。注意，千万不要去修改这个文件，我们在这里生成它纯粹是为了方便。

* ``mysite/mysite/``: 项目内部的这个 ``mysite/`` 目录是你的项目的一个Python包(package)。他的名字就是package的名字，你需要通过这个名字导入包里面的内容(比如， ``import mysite.settings``)。

* ``__init__.py``: 让Python把 ``mysite`` 目录当作一个包(一组Python模块)所必须的文件。这是一个空文件，通常情况下，不要往里面添加内容。

* ``settings.py``: 该Django项目的设置/配置。你可以先过目一下这个文件，可以了解到可以进行哪些配置，已经他们的默认值。

* ``urls.py``: Django项目的URL设置。这个文件相当于你的Django网站的目录。

* ``wsgi.py``: 是兼容WSGI的Web服务器伺服你的项目的入口文件。更多细节请查阅“如何通过WSGI部署”(https://docs.djangoproject.com/en/1.4/howto/deployment/wsgi/)。

尽管这些文件很小，但是这些文件已经构成了一个可运行的Djang程序。

运行开发服务器
------------------------------

为了让你对Django有更多的体验，我们来运行一个Django的开发服务器，来看看我们的已经有了些什么。

Django的开发服务器是（也被叫做“runserver”，这是来自于运行这个server的命令）一个用在你开发
期间的内建的轻量的Web服务器。我们提供这个服务器是为了让你能快速开发你的Web程序，在准备发布到生产环境
前，你都不需要去配置你生产环境的服务器。开发服务器会自动的检测代码的改变，并且自动加载它，这样，你修改代码后，
不需要去重启服务器。

切换到你项目的目录(``cd mysite``)，运行这个命令就可以启动开发服务器了：

::

    python manage.py runserver

你会看到像这样的信息：

::

    Validating models...
    0 errors found.

    Django version 1.4.2, using settings 'mysite.settings'
    Development server is running at http://127.0.0.1:8000/
    Quit the server with CONTROL-C.

这样，你的计算机上就有了一个监听8000端口，只接受从从你自己电脑上发出本地连接的服务器。服务器有了，
现在就可以用浏览器访问 http://127.0.0.1:8000/ 。你会看到一个令人赏心悦目的淡蓝色的Django欢迎
页面，不同的Django版本可能会有些差别，不过你会在页面上看到“Welcome to Django”的字样。It worked！


最后还要多提以下这个开发服务器。虽然Django自带的这个Web服务器对于开发很方便，但是，千万不要在生产环境用它。
这个服务器同一时间只能可靠地处理一个连接，而且也没有任何的安全检查。在发布你的站点前，请参阅 第12章_ 了解
如何部署Django。

.. admonition:: 更改开发服务器的主机地址或者端口

    默认情况下， ``runserver`` 命令会在8000端口启动一个开发服务器，仅仅监听本地连接。如果你想要
    更改服务器的端口，可以在命令行中指定端口：

    ::

        python manage.py runserver 8080

    也可以在命令中指定一个IP地址，你可以告诉服务器允许非本地连接。如果你想要和其他开发人员共享一个开发
    站点的时候，这个功能将会特别有用。 用 ``0.0.0.0`` 这个IP地址可以让服务器去侦听任意的网络接口

    ::

        python manage.py runserver 0.0.0.0:8000

    完成这些设置后，你局域网内的其他电脑就可以在浏览器中通过你的IP访问你的Django站点了，不如：
    http://192.168.1.103:8000/ 。（注意，你要检查以下你的网络配置，来查看你的IP地址。Unix
    用户可以在命令提示符中输入 ``ifconfig`` 来获取。Windows用户用 ``ipconfig`` 命令。）


下一章
============

好了，你已经安装好了所有的东西，开发服务器也跑起来了。下一章_ 我们就可以开始学习基础知识，用Django
来开发站点了。

.. _下一章: chapter03.html
.. _第12章: chapter12.html
