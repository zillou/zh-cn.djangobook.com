=============================
第三章: 视图和URL配置
=============================

上一章里我们介绍了如何创建一个Django项目并启动Django的开发服务器。本章你将学到用Django创建动态
网页的基本知识。


你的第一个Django页面：Hello World
===========================================

我们的第一个目标，创建一个网页，输出那个最著名的例子：“Hello world.”。

如果你曾经在没有Web框架的情况编写过一个简单的“Hello world”页面，你只要简单地在一个文本文件中输入“Hello world”
，将文件保存为 ``hello.html`` ，然后上传到你的Web服务器的一个目录中即可。不过注意，在这个过程中，你已经
指定了这个网页的两个关键信息：它的内容(字符串： ``Hello world`` )和它的URL( ``http://www.example.com/hello.html`` ，如果
你吧文件放在一个子目录中，也可能是 ``http://www.example.com/files/hello.html`` )。

在Django中，你也需要指定这两个信息，只不过得用不同的方法。网页的内容是靠视图函数(view function)来生成的，
URL则专门配置在URL配置中(URLconf)。首先，让我们来写我们的“Hello World” view function吧。

第一个视图
---------------

进入到上一章我们用 ``django-admin.py startproject`` 创建的 ``mysite`` 目录中。创建一个叫做
``views.py`` 的空文件。这个Python模块(module)将包含这一章我们将要编写的视图。这个 ``views.py`` 的名字
并没有特别的要求，Django并不要求它叫什么名字，不过根据管理，把它叫做 ``views.py`` 会让其他开发者更容易读懂
你的代码。

我们的“Hello world”视图很简单，完全是写定义函数，和导入包的语句。在 ``views.py`` 中输入这些内容：

::

    from django.http import HttpResponse

    def hello(request):
        return HttpResponse("Hello world")

让我们来逐句分析这段代码：

* 首先，我们从 ``django.http`` 中导入了 ``HttpResponse`` 这个类。我们会在接下来用到这个类。

* 接着，我们定义了一个叫做 ``hello`` 的视图函数(view function)。

  每个view function至少要有一个参数，通常被叫做 ``request`` 。这是一个对象，包含了触犯这个
  view function的Web请求的信息， 它是 ``django.http.HttpRequest`` 的一个示例(instance)。
  在这个例子中，虽然我们不对这个 ``request`` 做任何处理，但是我们还必须要把它作为视图的第一个参数。

  view function的名字并不重要；Django并不需要它以某种特定的方式命名。这里我们叫它 ``hello``
  是因为 ``hello`` 清晰地表明这个视图的用意。你当然也可以叫它 ``hello_wonderful_beautiful_world``
  或者其他差不多恶心的名字。下一小节“第一个URLconf”将会介绍Django如何找到这个函数。

* 这个函数只有一行简单的代码：它仅仅返回一个 ``HttpResponse`` 对象，这个对象包含了文本“Hello world”。

记住，一个视图本身就是一个Python的函数，这个函数接受一个 ``HttpRequest`` 作为它的第一个参数，并返回
一个 ``HttpResponse`` 的实例。要想让一个Python函数成为一个Django的视图，你需要让它满足这两个条件。
（也有例外，我们稍后会讲到。）

第一个URLconf
------------------

现在，如果你再运行 ``python manage.py runserver`` ，你还是会看到“Weclome to Django”的欢迎页面，
而看不到我们刚刚写的“Hello world”视图。因为我们的项目 ``mysite`` 还不知道有 ``hello`` 这个视图；
我们需要显式地告诉Django我们要让哪个URL来激活这个视图。 (这就像上面那个发布静态HTML的例子中，我们已经
创建了HTML文件，但是还没有把它上传到服务器上。) 在Django里，要关联view function到一个URL，需要用到
URLconf。

*URLconf* 就像是你的Django站点的目录。本质上，它是一个URL和这个URL将调用的view function之间的映射关系。
通过这种方式，你就可以告诉Django：“对于这个URL，调用这段代码，对于那个URL，调用那段代码。”例如，当用户访问
``/foo/`` 时，调用view function ``foo_view()`` ， 这个view function在 ``views.py`` 中。

在前一章，你执行 ``django-admin.py startproject`` 时，Django已经自动为你创建了一个
URLconf，就是 ``urls.py`` 那个文件。默认的 ``urls.py`` 会是下面这个样子：

::

    from django.conf.urls import patterns, include, url

    # Uncomment the next two lines to enable the admin:
    # from django.contrib import admin
    # admin.autodiscover()

    urlpatterns = patterns('',
        # Examples:
        # url(r'^$', 'mysite.views.home', name='home'),
        # url(r'^mysite/', include('mysite.foo.urls')),

        # Uncomment the admin/doc line below to enable admin documentation:
        # url(r'^admin/doc/', include('django.contrib.admindocs.urls')),

        # Uncomment the next line to enable the admin:
        # url(r'^admin/', include(admin.site.urls)),
    )

URLconf中带了一些被注释的功能，这些在Django中经常会被用到，所有你只要去掉那些功能的注释就可以
开启这些功能。下面是忽略掉被注释掉的内容后，``urls.py`` 中实际的内容：

::

    from django.conf.urls.defaults import patterns, include, url

    urlpatterns = patterns('',
    )

照例，我们来逐行分析这些代码：

* 第一行从 ``django.conf.urls.defaults`` 中导入了三个函数。这三个函数是Django URLconf的基本构造：
  ``patterns``, ``include`` 和 ``urls`` 。

* 第二行调用 ``patterns`` 这个函数，并把结果保存在一个叫做 ``urlpatterns`` 的变量中。 ``patterns``
  函数接受一个空字符串作为参数。(这个字符串被用作一个视图函数的通用前缀，我们会在 :doc:`chapter08` 深入介绍。)

这样要注意的是 ``urlpatterns`` 这个变量，Django会在你的URLconf模块中寻找它。这个变量定义了
URL和处理这个URL请求的代码的映射关系。默认情况下，URLconf是空白的，你的Django程序还是白板一块。
(如果你的URLconf是空的，Django就会认为这个项目是刚刚创建的，所以它会显示那个欢迎页面。)

要在URLconf中电价一个URL和view，只需要添加一个URL模式和view function的映射即可。这里演示如果
将我们的 ``hello`` 视图关联起来：

::

    from django.conf.urls.defaults import patterns, include, url
    from mysite.views import hello

    urlpatterns = patterns('',
        url(r'^hello/$', hello),
    )

(注意，为了简洁，我们移除了注释代码。如果你喜欢的话，你也可以保留那些行。)

我们做了两处修改：

* 首先，我们从模块 ``mysite/views.py`` 中导入了 ``hello`` 这个view function。Python的导入语法用
  ``mysite.views`` 表示 ``mysite/views.py`` 。(这段代码假设了 ``mysite/views.py`` 在Python的搜索路径中，下面的注释中会有详细介绍。)

* 接着，我们添加了 ``url(r'^hello/$', hello),`` 到 ``urlpatterns`` 中。这一行被叫做一个
  *URLpattern* 。 ``url()`` 函数会告诉Django如何处理你配置的url。 ``url()`` 的第一个参数是一个模式匹配字符串(
  正则表达式，稍后会有详细介绍)，第二个参数处理这个URLpattern的view function。 ``url()`` 还接受第三个参数，在
  :doc:`chapter08` 再详细介绍。

.. admonition:: 注意

  另一个值得注意的地方是，我们在正则表达式字符串前面加了一个 ``r`` 。在Python中，这表示一个原始字符串(raw
  string)，Python不过去转义那些反斜杠(``\``)。在普通的Python字符串中，反斜杠会被用来转义特殊字符，比如字符串
  ``'\n'`` 会被解释成换行符。当你加上 ``r`` 之后，Python就不会转义那些字符了， ``r'\n'`` 就是一个包括了一个反斜杠和一个小写字母“n”的字符串。正则表达式中反斜杠的用法和Python字符串中反斜杠的用法刚好冲突了。所以在Python中使用正则表达式的时候最好都加上 ``r`` 。本书中所有的URLpattern都会用这种形式。

简单来说，我们只是告诉了Django所有对URL ``/hello/`` 的请求都由 ``hello`` 这个view function来处理。

.. admonition:: Python的搜索路径(Python path)

    Python的搜索路径是指你用 ``import`` 导入语句时，Python所查找的系统目录清单。

    举例来说，如果你的Python路径设为 ``['',
    '/usr/lib/python2.7/site-packages', '/home/username/djcode']`` 。如果你执行语句
    ``from foo import bar`` 时，Python先在当前目录寻找叫 ``foo.py`` 的模块(Python路径中的第一个目录，用空字符串表示当前目录)。如果文件不存在，Python会继续查找
    ``/usr/lib/python2.7/site-packages/foo.py`` 。如果还是找不到，Python会接着寻找
    ``/home/username/djcode/foo.py`` 。最后，如果这个文件真的不存在是，Python会抛出异常 ``ImportError`` 。

    要检查你的Python路径，只需要在Python的交互解释器中输入：

    ::

        >>> import sys
        >>> print sys.path

    通常你都不需要去关心Python路径的设置，Python和Django会帮你处理好。
    (Django项目中Python路径的的设置是由 ``manage.py`` 控制的)。

讨论一下URLpattern的语法也是很有必要的，因为它并不那么直观。我们是想要匹配地址 ``/hello`` ，
但是URLpattern里面的匹配模式(pattern)却有些不一样。下面我们来详细介绍一下：

* Django在检查URL模式之前，会移除每个URL最开始的斜杠。这意味着URLpattern并不包括 ``/hello`` 开头那个斜杠。(一开始，这可能不太直观，但是这样做简化了很多事情，我们会在 :doc: chapter08 里详细介绍。)

* 匹配模式里面包括了一个脱字符( ``^`` )和一个美元符( ``$`` )。这两个符号在正则表达式里面的有特别的意义： ``^`` 要求从字符串开头开始匹配， ``$`` 则是要求对字符串尾部进行匹配。

  最好还是用实例来说明。如果我们不用尾部匹配符 ``$`` ，所有以 ``/hello/`` 开头的URL都可以匹配，比如
  ``/hello/foo`` 或者 ``/hello/bar`` 和 ``/hello`` 。类似的，如果我们省掉头部匹配符
  ``^`` ，Django会匹配所有以 ``hello/`` 结尾的URL，比如 ``/foo/bar/hello/`` 。如果我们同时省掉
  这两个字符的话，只要包含了 ``hello/`` 的URL都将会匹配， 比如 ``/foo/hello/bar`` 。所以，加上
  ``^`` 和 ``$`` 是为了保证只有 ``/hello/`` 匹配，不多也不少。

  大多数的URLpattern都会以一个脱字符( ``^`` )开头，以一个美元符( ``$`` )结尾。
  但是拥有匹配复杂URL的灵活性会也很好。

  你也许会问，如果由人访问 ``/hello`` (尾部没有斜杠)会怎样，因为我们的URL模式要求结尾
  有一个斜杠，这样这个URL并不匹配我们定义的模式。不过，默认情况下，一个没有以斜杠结尾的URL
  找不到匹配的URLpattern的话，会被重定向(redirect)到一个添加了斜杠的相同URL去。(者是受Django
  设置里面的 ``APPEND_SLASH`` 选项控制的，参见 附录D_ 。)

  如果你是喜欢所有的URL都以 ``/`` 结尾的人(Django开发者都喜欢这样)。你只需要在每个URL
  后添加斜杠并且设置 ``APPEND_SLASH`` 为 ``True`` 。如果你更愿意不要结尾的那个斜杠，
  或者根据每个URL的情况来决定的话，那么需要将 ``APPEND_SLASH`` 设置为 ``False`` ，
  然后根据你的意愿来添加结尾的斜杠到你想要添加的URL上。

另一个要注意的地方是。这个URLconf中我们是把 ``hello`` 这个view function直接作为一个对象传递的，而不是去调用它。
这是Pyhton(作为动态语言)的一个重要特征，函数是一级对象(first-class objects)，你可以像
传递其它变量一样传递一个函数。很酷吧？

要检查我们配置的URLconf的话，如同第二章一样， 用 ``python manage.py runserver``
启动Django的开发服务器。(如果你还没有关掉之前启动的那个，就不用再启动一个了。Django开发
服务器会自动检测到你做的更改，然后自动加载它们，不需要你去重启它。) 开发服务器会运行在
``http://127.0.0.1:8000/`` ，打开一个浏览器，访问
``http://127.0.0.1:8000/hello/`` 。你应该会看到你的Django视图输出的文本--“Hello world”。

耶! 你用Django成功创建了你的第一个网页了。

.. admonition:: 正则表达式

    正则表达式(*Regular expressions* 或者 *regexes* )是通用的文本模式匹配方法。
    Django的URLconf运行你使用任意的正则表达式来做URL匹配，不过实际情况中，你通常只需要使用到其中很少的一部分功能。下面是写基本的语法。

    ============  ==========================================================
    符号           匹配
    ============  ==========================================================
    ``.`` (dot)   匹配任一字符

    ``\d``        匹配任意一个数字

    ``[A-Z]``     匹配A-Z中的任意一个大写字母(即匹配任意一个大写的英文字母)

    ``[a-z]``     匹配A-Z中的任意一个小写字母(即匹配任意一个小写的英文字母)

    ``[A-Za-z]``  匹配a-z中任意一个字母(不区分大小写)

    ``+``         匹配一个或更多其前面的表达式 (例如： ``\d+`` 匹配一个或多个数字)

    ``[^/]+``     匹配一个或多个不为‘/’的字符

    ``?``         匹配 **零个** 或多个其前面的表达式(例如： ``\d?`` 匹配0个或1个数字)

    ``*``         匹配 **零个** 或多个其前面的表达式(例如： ``\d*`` 匹配0个，1个或多个数字)

    ``{1,3}``     介于1个和3个之前的表达式(例如： ``\d{1,3}`` 匹配1个，2个或者3个数字)
    ============  ==========================================================

    更多关于正则表达式的内容，请查阅 http://www.djangoproject.com/r/python/re-module/ 。

404错误简介
-----------------------------

目前，我们的URLconf中只定义了一个URLpattern：处理到 ``/hello/`` 的请求。如果请求其他的URL会怎么样呢？

运行你的Django开发服务器，然后访问一个类似 ``http://127.0.0.1:8000/goodbye/``, ``http://127.0.0.1:8000/hello/subdirectory/`` 或者是 ``http://127.0.0.1:8000/`` (网站根目录)的页面。你会看到一个“Page
not found”信息(如图3-1所示)。因为你没有定义如何处理这个URL请求，所有Django显示这样的信息。

.. figure:: graphics/chapter03/404.png
   :alt: Django的404页面截图.

   图3-1. Django的404页面

这个页面比原始的404错误信息更加实用，它还显示了哪个URLconf被用到，以及这个URLconf里面的
每一个匹配模式。你可以通过这个信息知道为什么你请求的URL会抛出404错误。

当然，这些敏感的信息只会显示给开发者，如果是已经部署到因特网上的生产站点就不能再将这些信息暴露出来。
所以，这个“Page not found”的页面只会在 *调试模式* 下显示，我们会在以后说明怎么关闭调试模式，
你现在只要知道，每个项目创建时默认是处在调试模式的，如果关闭调试模式后，Django会输出一个不同的
404响应。


网站根目录简介
--------------------------------

上一节中，我们访问网站根目录 ``http://127.0.0.1:8000/`` 时，得到一个404错误消息。
Django不会在你的网站根目录下增加任何东西，Django不会区别对待这个URL。这需要你到URLconf中
为它指定一个URLpattern，和URLconf中其他条目完全一样。

要去匹配网站根目录可能不是那么直观，所以这里专门列出来一下。当你写好你的网站根目录的视图之后，
用 ``'^$'`` 这个模式去匹配，因为它会匹配一个空字符串。如下面的例子所示：

::

    from mysite.views import hello, my_homepage_view

    urlpatterns = patterns('',
        url(r'^$', my_homepage_view),
        # ...
    )

Django如何处理一个请求
==============================

在开始我们的第二个视图之前，我想暂停一下，先去了解一点Django的原理。当你通过浏览器访问
``http://127.0.0.1:8000/hello/`` ，你会看到你的“Hello world”信息。Django在后台
都做了哪些事情呢？

所有均开始于setting文件。当你运行 ``python manage.py runserver`` 时，这个脚本会去
内层那个 ``mysite`` 文件夹内查找一个 ``setting.py`` 的文件。这个文件包裹了当前这个Django
项目的各种各样的配置。这些配置信息都是大写的，如： ``TEMPLATE_DIRS`` ，``DATABASES`` 等等。
最重要的一个配置是 ``ROOT_URLCONF`` 。 ``ROOT_URLCONF`` 定义了那个Python模块被用在这个网站
的URLconf。

还记得运行 ``django-admin.py startproject`` 的时候创建了 ``settings.py`` 和 ``urls.py``
这两个文件吧？这个自动创建的 ``settings.py`` 包括了 ``ROOT_URLCONF`` ，并且将其指向了自动生产的
``settings.py`` 。打开这个 ``setting.py`` ，你会看到：

::

    ROOT_URLCONF = 'mysite.urls'

其对应的文件就是 ``mysite/urls.py`` 。

当你访问一个特定的URL时，比如 ``/hello/`` ， Django会加载 ``ROOT_URLCONF`` 指向的URLconf，
然后按顺序逐个检查里面的URLpattern，知道找到和请求的URL匹配的URLpattern。当找到匹配的URLpattern后，
Django会调用和这个URLpattern相关联的view function，并把当前的请求作为一个 ``HttpRequest`` 对象
传到view function的第一个参数。(稍后我们会介绍 ``HttpRequest`` 的标准。)

正如我们的第一个例子一样，一个view function必须返回一个 ``HttpResponse`` 对象。剩下的事情就由Django
来完成，Django会把这个Pythong对象转换成合适的Web响应，设置合适的HTTP头部以及内容(比如网页的内容)。

总结一下：

1. 一个请求进来，请求 ``/hello/`` 。
2. Django通过 ``ROOT_URLCONF`` 找到根URLconf。
3. Django在URLconf中找到第一个匹配 ``/hello/`` 的URLpattern。
4. 如果找到匹配项，Django调用对应的视图函数。
5. 视图函数返回一个 ``HttpResponse`` 。
6. Django将 ``HttpResponse`` 转换成正确的HTTP响应，就是浏览器中显示的网页了。

你现在知道怎么做一个Django页面了。真的很简单，只需要编写一些view function，然后通过URLconf将他们
和URL一一对应起来。


第二个视图： 动态内容
=================================

我们的“Hello world”视图是用来演示Django的工作方式，它还不是一个 *动态* 的网页，因为
它的内容总是一样的。每次你访问 ``/hello/`` 都会看到一样的内容，这还是和一个静态HTML文件
一样。

那让我们的第二个视图更动态一点儿吧，我们还做一个显示当前日期和时间的页面。这个例子足够简单，
不涉及数据库或者用户输入，仅仅是将服务器上的时间显示到页面上。这个例子只是比上面那个稍微有
意思一点，不过它也能用来展示几个概念。

这个视图需要做两件事：计算当前的日期和时间，以及返回一个包含这个值的 ``HttpResponse`` 。
如果你有过Python的经验，你也许知道Python有一个 ``datetime`` 模块可以用来计算日期。
下面我们来看看如何用它：

::

    >>> import datetime
    >>> now = datetime.datetime.now()
    >>> now
    datetime.datetime(2008, 12, 13, 14, 9, 39, 2731)
    >>> print now
    2008-12-13 14:09:39.002731

很简单，也并没有涉及到Django。它仅仅是Python的代码。(我们希望你注意哪些是纯Python代码，哪些
是有Django特性的代码，这样，在你学习Django的过程中，你也可以学到一些Python的知识，并用到其他的
不用Django的Python项目中。)

To make a Django view that displays the current date and time, then, we just
need to hook this ``datetime.datetime.now()`` statement into a view and return
an ``HttpResponse``. Here's how that looks::

    from django.http import HttpResponse
    import datetime

    def current_datetime(request):
        now = datetime.datetime.now()
        html = "<html><body>It is now %s.</body></html>" % now
        return HttpResponse(html)

As with our ``hello`` view function, this should live in ``views.py``. Note
that we've hidden the ``hello`` function from this example for brevity, but for
the sake of completeness, here's what the entire ``views.py`` looks like::

    from django.http import HttpResponse
    import datetime

    def hello(request):
        return HttpResponse("Hello world")

    def current_datetime(request):
        now = datetime.datetime.now()
        html = "<html><body>It is now %s.</body></html>" % now
        return HttpResponse(html)

(From now on, we won't display previous code in code examples, except when
necessary. You should be able to tell from context which parts of an example
are new vs. old.)

Let's step through the changes we've made to ``views.py`` to accommodate
the ``current_datetime`` view.

* We've added an ``import datetime`` to the top of the module, so we can
  calculate dates.

* The new ``current_datetime`` function calculates the current date and
  time, as a ``datetime.datetime`` object, and stores that as the local
  variable ``now``.

* The second line of code within the view constructs an HTML response using
  Python's "format-string" capability. The ``%s`` within the string is a
  placeholder, and the percent sign after the string means "Replace the
  ``%s`` in the preceding string with the value of the variable ``now``."
  The ``now`` variable is technically a ``datetime.datetime`` object, not
  a string, but the ``%s`` format character converts it to its string
  representation, which is something like ``"2008-12-13 14:09:39.002731"``.
  This will result in an HTML string such as
  ``"<html><body>It is now 2008-12-13 14:09:39.002731.</body></html>"``.

  (Yes, our HTML is invalid, but we're trying to keep the example simple
  and short.)

* Finally, the view returns an ``HttpResponse`` object that contains the
  generated response -- just as we did in ``hello``.

After adding that to ``views.py``, add the URLpattern to ``urls.py`` to tell
Django which URL should handle this view. Something like ``/time/`` would make
sense::

    from django.conf.urls.defaults import patterns, include, url
    from mysite.views import hello, current_datetime

    urlpatterns = patterns('',
        url(r'^hello/$', hello),
        url(r'^time/$', current_datetime),
    )

We've made two changes here. First, we imported the ``current_datetime``
function at the top. Second, and more importantly, we added a URLpattern
mapping the URL ``/time/`` to that new view. Getting the hang of this?

With the view written and URLconf updated, fire up the ``runserver`` and visit
``http://127.0.0.1:8000/time/`` in your browser. You should see the current
date and time.

.. admonition:: Django's Time Zone

    Depending on your computer, the date and time may be a few hours off.
    That's because Django is time zone-aware and defaults to the
    ``America/Chicago`` time zone. (It has to default to *something*, and that's
    the time zone where the original developers live.) If you live elsewhere,
    you'll want to change it in ``settings.py``. See the comment in that file
    for a link to an up-to-date list of worldwide time zone options.

URLconfs and Loose Coupling
===========================

Now's a good time to highlight a key philosophy behind URLconfs and behind
Django in general: the principle of *loose coupling*. Simply put, loose coupling
is a software-development approach that values the importance of making pieces
interchangeable. If two pieces of code are loosely coupled, then changes made to
one of the pieces will have little or no effect on the other.

Django's URLconfs are a good example of this principle in practice. In a Django
web application, the URL definitions and the view functions they call are
loosely coupled; that is, the decision of what the URL should be for a given
function, and the implementation of the function itself, reside in two separate
places. This lets you switch out one piece without affecting the other.

For example, consider our ``current_datetime`` view. If we wanted to change the
URL for the application -- say, to move it from ``/time/`` to
``/current-time/`` -- we could make a quick change to the URLconf, without
having to worry about the view itself. Similarly, if we wanted to change the
view function -- altering its logic somehow -- we could do that without
affecting the URL to which the function is bound.

Furthermore, if we wanted to expose the current-date functionality at
*several* URLs, we could easily take care of that by editing the URLconf,
without having to touch the view code. In this example, our
``current_datetime`` is available at two URLs. It's a contrived example, but
this technique can come in handy::

    urlpatterns = patterns('',
        url(r'^hello/$', hello),
        url(r'^time/$', current_datetime),
        url(r'^another-time-page/$', current_datetime),
    )

URLconfs and views are loose coupling in action. We'll continue to point out
examples of this important philosophy throughout this book.

Your Third View: Dynamic URLs
=============================

In our ``current_datetime`` view, the contents of the page -- the current
date/time -- were dynamic, but the URL (``/time/``) was static. In most dynamic
Web applications, though, a URL contains parameters that influence the output
of the page. For example, an online bookstore might give each book its own URL,
like ``/books/243/`` and ``/books/81196/``.

Let's create a third view that displays the current date and time offset by a
certain number of hours. The goal is to craft a site in such a way that the page
``/time/plus/1/`` displays the date/time one hour into the future, the page
``/time/plus/2/`` displays the date/time two hours into the future, the page
``/time/plus/3/`` displays the date/time three hours into the future, and so
on.

A novice might think to code a separate view function for each hour offset,
which might result in a URLconf like this::

    urlpatterns = patterns('',
        url(r'^time/$', current_datetime),
        url(r'^time/plus/1/$', one_hour_ahead),
        url(r'^time/plus/2/$', two_hours_ahead),
        url(r'^time/plus/3/$', three_hours_ahead),
        url(r'^time/plus/4/$', four_hours_ahead),
    )

Clearly, this line of thought is flawed. Not only would this result in redundant
view functions, but also the application is fundamentally limited to supporting
only the predefined hour ranges -- one, two, three or four hours. If we decided
to create a page that displayed the time *five* hours into the future, we'd
have to create a separate view and URLconf line for that, furthering the
duplication. We need to do some abstraction here.

.. admonition:: A Word About Pretty URLs

    If you're experienced in another Web development platform, such as PHP or
    Java, you may be thinking, "Hey, let's use a query string parameter!" --
    something like ``/time/plus?hours=3``, in which the hours would be
    designated by the ``hours`` parameter in the URL's query string (the part
    after the ``?``).

    You *can* do that with Django (and we'll tell you how in Chapter 7), but
    one of Django's core philosophies is that URLs should be beautiful. The URL
    ``/time/plus/3/`` is far cleaner, simpler, more readable, easier to recite
    to somebody aloud and . . . just plain prettier than its query string
    counterpart. Pretty URLs are a characteristic of a quality Web application.

    Django's URLconf system encourages pretty URLs by making it easier to use
    pretty URLs than *not* to.

How, then do we design our application to handle arbitrary hour offsets? The
key is to use *wildcard URLpatterns*. As we mentioned previously, a URLpattern
is a regular expression; hence, we can use the regular expression pattern
``\d+`` to match one or more digits::

    urlpatterns = patterns('',
        # ...
        url(r'^time/plus/\d+/$', hours_ahead),
        # ...
    )

(We're using the ``# ...`` to imply there might be other URLpatterns that we
trimmed from this example.)

This new URLpattern will match any URL such as ``/time/plus/2/``,
``/time/plus/25/``, or even ``/time/plus/100000000000/``. Come to think of it,
let's limit it so that the maximum allowed offset is 99 hours. That means we
want to allow either one- or two-digit numbers -- and in regular expression
syntax, that translates into ``\d{1,2}``::

    url(r'^time/plus/\d{1,2}/$', hours_ahead),

.. note::

    When building Web applications, it's always important to consider the most
    outlandish data input possible, and decide whether or not the application
    should support that input. We've curtailed the outlandishness here by
    limiting the offset to 99 hours.

Now that we've designated a wildcard for the URL, we need a way of passing that
wildcard data to the view function, so that we can use a single view function
for any arbitrary hour offset. We do this by placing parentheses around the
data in the URLpattern that we want to save. In the case of our example, we
want to save whatever number was entered in the URL, so let's put parentheses
around the ``\d{1,2}``, like this::

    url(r'^time/plus/(\d{1,2})/$', hours_ahead),

If you're familiar with regular expressions, you'll be right at home here;
we're using parentheses to *capture* data from the matched text.

The final URLconf, including our previous two views, looks like this::

    from django.conf.urls.defaults import *
    from mysite.views import hello, current_datetime, hours_ahead

    urlpatterns = patterns('',
        url(r'^hello/$', hello),
        url(r'^time/$', current_datetime),
        url(r'^time/plus/(\d{1,2})/$', hours_ahead),
    )

With that taken care of, let's write the ``hours_ahead`` view.

``hours_ahead`` is very similar to the ``current_datetime`` view we wrote
earlier, with a key difference: it takes an extra argument, the number of hours
of offset. Here's the view code::

    from django.http import Http404, HttpResponse
    import datetime

    def hours_ahead(request, offset):
        try:
            offset = int(offset)
        except ValueError:
            raise Http404()
        dt = datetime.datetime.now() + datetime.timedelta(hours=offset)
        html = "<html><body>In %s hour(s), it will be %s.</body></html>" % (offset, dt)
        return HttpResponse(html)

Let's step through this code one line at a time:

* The view function, ``hours_ahead``, takes *two* parameters: ``request``
  and ``offset``.

  * ``request`` is an ``HttpRequest`` object, just as in ``hello`` and
    ``current_datetime``. We'll say it again: each view *always* takes an
    ``HttpRequest`` object as its first parameter.

  * ``offset`` is the string captured by the parentheses in the
    URLpattern. For example, if the requested URL were ``/time/plus/3/``,
    then ``offset`` would be the string ``'3'``. If the requested URL were
    ``/time/plus/21/``, then ``offset`` would be the string ``'21'``. Note
    that captured values will always be *strings*, not integers, even if
    the string is composed of only digits, such as ``'21'``.

    (Technically, captured values will always be *Unicode objects*, not
    plain Python bytestrings, but don't worry about this distinction at
    the moment.)

    We decided to call the variable ``offset``, but you can call it
    whatever you'd like, as long as it's a valid Python identifier. The
    variable name doesn't matter; all that matters is that it's the second
    argument to the function, after ``request``. (It's also possible to
    use keyword, rather than positional, arguments in an URLconf. We cover
    that in Chapter 8.)

* The first thing we do within the function is call ``int()`` on ``offset``.
  This converts the string value to an integer.

  Note that Python will raise a ``ValueError`` exception if you call
  ``int()`` on a value that cannot be converted to an integer, such as the
  string ``'foo'``. In this example, if we encounter the ``ValueError``, we
  raise the exception ``django.http.Http404``, which, as you can imagine,
  results in a 404 "Page not found" error.

  Astute readers will wonder: how could we ever reach the ``ValueError``
  case, anyway, given that the regular expression in our URLpattern --
  ``(\d{1,2})`` -- captures only digits, and therefore ``offset`` will only
  ever be a string composed of digits? The answer is, we won't, because
  the URLpattern provides a modest but useful level of input validation,
  *but* we still check for the ``ValueError`` in case this view function
  ever gets called in some other way. It's good practice to implement view
  functions such that they don't make any assumptions about their
  parameters. Loose coupling, remember?

* In the next line of the function, we calculate the current date/time and
  add the appropriate number of hours. We've already seen
  ``datetime.datetime.now()`` from the ``current_datetime`` view; the new
  concept here is that you can perform date/time arithmetic by creating a
  ``datetime.timedelta`` object and adding to a ``datetime.datetime``
  object. Our result is stored in the variable ``dt``.

  This line also shows why we called ``int()`` on ``offset`` -- the
  ``datetime.timedelta`` function requires the ``hours`` parameter to be an
  integer.

* Next, we construct the HTML output of this view function, just as we did
  in ``current_datetime``. A small difference in this line from the previous
  line is that it uses Python's format-string capability with *two* values,
  not just one. Hence, there are two ``%s`` symbols in the string and a
  tuple of values to insert: ``(offset, dt)``.

* Finally, we return an ``HttpResponse`` of the HTML. By now, this is old
  hat.

With that view function and URLconf written, start the Django development server
(if it's not already running), and visit ``http://127.0.0.1:8000/time/plus/3/``
to verify it works. Then try ``http://127.0.0.1:8000/time/plus/5/``. Then
``http://127.0.0.1:8000/time/plus/24/``. Finally, visit
``http://127.0.0.1:8000/time/plus/100/`` to verify that the pattern in your
URLconf only accepts one- or two-digit numbers; Django should display a "Page
not found" error in this case, just as we saw in the section "A Quick Note
About 404 Errors" earlier. The URL ``http://127.0.0.1:8000/time/plus/`` (with
*no* hour designation) should also throw a 404.

.. admonition:: 编码顺序

    这个例子中，我们先写了URLpattern，然后才写视图，但是上一个例子中我们是先写视图，才是URLpattern。
    哪一种方式更好呢？

    嗯，怎么说呢，每个开发者都不一样。

    如果你是一个喜欢从总体上来把握的人，你应该更喜欢在项目开始的时候就写下所有的URL配置。
    然后再去编写每个对应的视图。这种方式的一个好处是它会给你一个很清晰的列表，一个to-do list，
    还明确地定义了你需要编写的视图函数的参数。

    如果你更像一个自底向上的开发者，你可能更喜欢先写视图，然后把他们和URL联系起来。这样也是没问题的。

    最后，这两种方式都是可行的，取决于你的大脑更适应哪种思考方式。


Django漂亮的出错页面
===========================

哈，让我花点时间来欣赏一下我们写好的Web程序吧……然后，我们来搞点儿小破坏。我们来把 ``offset = int(offset)``
注释掉，这会让 ``views.py`` 中产生一个错误。

::

    def hours_ahead(request, offset):
        # try:
        #     offset = int(offset)
        # except ValueError:
        #     raise Http404()
        dt = datetime.datetime.now() + datetime.timedelta(hours=offset)
        html = "<html><body>In %s hour(s), it will be %s.</body></html>" % (offset, dt)
        return HttpResponse(html)

启动开发服务器，然后访问 ``/time/plus/3/`` 。 你会看到一个包含大量信息的出错页，最上面是一条 ``TypeError``
信息 ``"unsupported type for timedelta hours component: unicode"`` 。

怎么回事呢？ ``datetime.timedelta`` 函数要求参数 ``hours`` 是一个整数，但是我们注释掉
了将 ``offset`` 转换为整数的代码。这就导致了 ``datetime.timedelta`` 抛出 ``TypeError``
的异常。这是每个程序员都会遇到的小bug。

这个例子是为了展示Django的出错页面。我们花点时间来看看这个出错页，看看里面有哪些
信息。

以下几点值得注意：

* 在页面顶部，你会得到关键的异常信息：异常类型，造成异常的参数(如本例中的
  ``"unsupported type"`` 信息)，异常发生在哪个文件，出错的行号等等。

* Under the key exception information, the page displays the full Python
  traceback for this exception. This is similar to the standard traceback
  you get in Python's command-line interpreter, except it's more
  interactive. For each level ("frame") in the stack, Django displays the
  name of the file, the function/method name, the line number, and the
  source code of that line.

  Click the line of source code (in dark gray), and you'll see several
  lines from before and after the erroneous line, to give you context.

  Click "Local vars" under any frame in the stack to view a table of all
  local variables and their values, in that frame, at the exact point in the
  code at which the exception was raised. This debugging information can be
  a great help.

* Note the "Switch to copy-and-paste view" text under the "Traceback"
  header. Click those words, and the traceback will switch to a alternate
  version that can be easily copied and pasted. Use this when you want to
  share your exception traceback with others to get technical support --
  such as the kind folks in the Django IRC chat room or on the Django users
  mailing list.

  Underneath, the "Share this traceback on a public Web site" button will
  do this work for you in just one click. Click it to post the traceback to
  http://www.dpaste.com/, where you'll get a distinct URL that you can
  share with other people.

* Next, the "Request information" section includes a wealth of information
  about the incoming Web request that spawned the error: GET and POST
  information, cookie values, and meta information, such as CGI headers.
  Appendix G has a complete reference of all the information a request
  object contains.

  Below the "Request information" section, the "Settings" section lists all
  of the settings for this particular Django installation. (We've already
  mentioned ``ROOT_URLCONF``, and we'll show you various Django settings
  throughout the book. All the available settings are covered in detail in
  Appendix D.)

The Django error page is capable of displaying more information in certain
special cases, such as the case of template syntax errors. We'll get to those
later, when we discuss the Django template system. For now, uncomment the
``offset = int(offset)`` lines to get the view function working properly again.

Are you the type of programmer who likes to debug with the help of carefully
placed ``print`` statements? You can use the Django error page to do so -- just
without the ``print`` statements. At any point in your view, temporarily insert
an ``assert False`` to trigger the error page. Then, you can view the local
variables and state of the program. Here's an example, using the
``hours_ahead`` view::

    def hours_ahead(request, offset):
        try:
            offset = int(offset)
        except ValueError:
            raise Http404()
        dt = datetime.datetime.now() + datetime.timedelta(hours=offset)
        assert False
        html = "<html><body>In %s hour(s), it will be %s.</body></html>" % (offset, dt)
        return HttpResponse(html)

Finally, it's obvious that much of this information is sensitive -- it exposes
the innards of your Python code and Django configuration -- and it would be
foolish to show this information on the public Internet. A malicious person
could use it to attempt to reverse-engineer your Web application and do nasty
things. For that reason, the Django error page is only displayed when your
Django project is in debug mode. We'll explain how to deactivate debug mode
in Chapter 12. For now, just know that every Django project is in debug mode
automatically when you start it. (Sound familiar? The "Page not found" errors,
described earlier in this chapter, work the same way.)

下一章
============

本章里，我们编写了view function，把HTML直接hard-code在Python代码里了。我们这样做是为了
演示方便，但是在实际情况中，一般这样的方式都不好。

Djano提供了一个简单但强大的模板引擎，可以让你将页面的设计和底层的代码分隔开来。下一章_ ，我们就将
深入Django的模板引擎。

.. _下一章: chapter04.html
.. _附录D: appendixD.html
