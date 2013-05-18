====================
第一章： 介绍Django
====================

这本书是关于Django。Django是一个可以是Web开发工作更加高效愉快的Web开发框架。Django可以
让你用最小的代价构建和维护更高质量的Web应用程序。

从好的方面来看，Web开发是一项有趣和充满创造性的事；但是另一方面，Web开发又可能是一项繁琐令人生厌的工作。
通过减少重复的部分，Django能让你专注于Web程序的核心，从而让开发更加有趣。
为了达到这个目标，Django提供了通用Web开发模式的高度抽象，一些常见任务的快捷方式，还为“如何解决问题”提供了
清晰明了的约定。同时，Django尝试留下一些方法，来让你根据自己的需要在framework之外来开发。

本书的目标就是将你培养成Django专家。将主要侧重在两方面：第一， 我们深度解析Django到底做了哪些事情，以及如何
用Django来构建Web程序。第二，我们会在合适的时机讨论一些更高级的概念，并解释如何在你自己的项目中高效地利用这些工具。
通过阅读此书，你将学会快速开发一个功能强大的网站的技巧，并且你的代码也将会十分清晰，易于维护。

Web框架是什么?
===============

Django是新一代 *Web框架* 中的佼佼者，但是，Web框架到底是什么？

要回答这个问题，首先让我们来思考一下 *不要* Web框架如何用Python来设计一个Web程序。贯穿本书，我们
会继续用这样的方法，先向你展示如何不通过框架来实现功能的基本方法，让你认识到框架能够带来的便利。
（同时知道如何离开框架去解决问题也是非常有用的，因为有些时候，框架并没有提供合适的方法去解决你的问题。
更重要的是，理解实现的来龙去脉会让你成为更好的Web开发者。）

使用Python开发Web应用，最简单，同时也是最原始的方法是使用 CGI_ (Comman Gateway Interface)标准,
在1998年，这种方式很流行。它是这样工作的：创建一个Python脚本，输出HTML代码，然后将脚本保存到Web服务器上，把脚本后缀名改为“.cgi”就可以了。

这里是一个用Python CGI脚本从数据库中读出最近出版的十本书，并将其列出来的例子。先不用担心看不懂语法细节，只需要大概了解一些这段代码在做什么。 ::

    #!/usr/bin/env python

    import MySQLdb

    print "Content-Type: text/html\n"
    print "<html><head><title>Books</title></head>"
    print "<body>"
    print "<h1>Books</h1>"
    print "<ul>"

    connection = MySQLdb.connect(user='me', passwd='letmein', db='my_db')
    cursor = connection.cursor()
    cursor.execute("SELECT name FROM books ORDER BY pub_date DESC LIMIT 10")

    for row in cursor.fetchall():
        print "<li>%s</li>" % row[0]

    print "</ul>"
    print "</body></html>"

    connection.close()

首先，为了满足CGI标准，这段代码要先打印一行“Content-Type”，接着是一个空白行。接着打印一些HTML
的开始标签，然后连接数据库，执行一些查询，取到最近出版的10本书。再遍历这些书，生成列出书名的HTML。
最后，输出HTML的关闭代码，并且断开数据库连接。

对于这样的一次性动态页面，从头写起也并非不好，至少，这些代码简单易懂，就算一个初学者都能读懂这16行Python
并且对这些代码做的事情一清二楚。不需要学习额外的背景知识，也没有额外的代码需要去理解。部署也很容易，只需要将代码
保存成后缀是“.cgi”的文件，上传到服务器上，通过浏览器访问就可以了。

但是，尽管这种实现很简单，但是这种方法有几个问题。首先，问你自己这几个问题：

* 如果你的程序中多处都需要连接数据库会怎样？明显的，我们不需要在每个CGI中重复数据库连接的代码,
  最好将它写到一个共享函数里面。

* 真的要让一个程序员需要去关心如何输出“Content-Type”，以及每次都要记住关闭数据库连接？
  通常这类问题只会降低程序员的效率，增加错误的概率。那些初始化和释放的工作都应该由一些通用的
  代码来完成。

* 如果这些代码会被用到其他的环境，每个环境都需要单独的数据库连接信息？
  所以，方便各个环境单独配置也是很重要的部分。

* 如果一个Web设计师，完全没有Python的开发经验，但是又需要重新设计页面？
  一个字符写错就可能导致整个程序崩溃。理想的情况是，从数据库中取到图书条目的逻辑需要和
  在用HTML展示的代码分开。这样，Web设计师就可以安心的修改页面的HTML。

以上这些问题正是一个Web框架要解决的问题。一个Web框架为你的程序提供了一套程序基础框架，这样你
可以专注在编写清晰，易于维护的代码上，而不需要去从头发明另一个轮子。简单来说。这就是Django做的事情。

MVC设计模式
======================

让我们来研究一个简单的例子，通过这个实例，你可以看出用Web框架方式与之前的做法有什么不同。
下面就是如果通过Django来完成上面那个例子。首先，我们将内容分隔到3个Python的文件中（ ``models.py`` ``views.py`` ``urls.py`` 和一个模板文件（ ``latest_books.html`` ）。

::

    # models.py (the database tables)

    from django.db import models

    class Book(models.Model):
        name = models.CharField(max_length=50)
        pub_date = models.DateField()


    # views.py (the business logic)

    from django.shortcuts import render
    from models import Book

    def latest_books(request):
        book_list = Book.objects.order_by('-pub_date')[:10]
        return render(request, 'latest_books.html', {'book_list': book_list})


    # urls.py (the URL configuration)

    from django.conf.urls.defaults import *
    import views

    urlpatterns = patterns('',
        (r'^latest/$', views.latest_books),
    )


    # latest_books.html (the template)

    <html><head><title>Books</title></head>
    <body>
    <h1>Books</h1>
    <ul>
    {% for book in book_list %}
    <li>{{ book.name }}</li>
    {% endfor %}
    </ul>
    </body></html>

同样，不需要太专注语法细节，只需要注意这里实现的方式。
这里要重点提到的是 *重点分离* （separation of concerns）：

* ``models.py`` 中用一个Python类来描述一个数据库中的表，被称作 *模型* (model)。
  通过这个类，你可以通过简单的Python代码来对数据库中的记录进行增删改查（创建，检索，更新，删除），
  而无需你去写一条一条的SQL语句。

* ``views.py`` 中包含了页面的业务逻辑。 ``latest_books()`` 方法叫做 *视图* (view)。

* ``urls.py`` 指定视图和URL的关系，即什么样的URL调用哪个的视图。在这个例子中， ``/lastest/`` URL
  会调用 ``latest_books()`` 这个方法。换句话说如果你的域名是example.com，任何人访问http://example.com/latest/
  就会调用 ``latest_books()`` 。

* ``latest_books.html`` 是HTML模板，它定义了这个页面的设计，模板中使用了带基本逻辑语句的模板语言，
  比如 ``{% for book in book_list %}`` 。

这部分代码松散地遵循了模型-视图-控制器(MVC)的设计模式。简单来说，MVC是一种软件开发的方法，它把定义和访问
数据的代码(模型 model)和控制请求逻辑的代码(控制器 controller)和用户接口(视图 view)分割开来。我们会在 第五章_ 更加深入地讨论MVC。

这个方法最重要的优点在于它的各个部分都是 *松耦合* (loosely coupled)的。这样用Django开发的Web程序
中每个部分都有它自己单一的目的，并且可以单独地被修改而不会影响到其他部分。比方说，一个程序员可以在不影响
底层实现的情况下修改了URL；设计师可以不需要接触Python的代码就修改页面的HTML；数据库管理员在重命名
数据表之后只需要修改一个地方就可以了，而不需要在一大堆文件中查找替换。

本书中，我们会分出单独的章节来分别讲解MVC中的各个部分。第三章介绍视图，第四章介绍模板，第五章是模型。

Django的历史
=============

在我们讨论代码之前我们需要先花点实践介绍一下Django的历史。前面我们向你介绍了如何在不用框架提供的快捷方式的情况
下完成工作，以便你能更好的理解这种快捷方式。同样，了解Django产生的背景也有助于你理解Django的实现方式。

如果你曾经编写过Web程序，你可能对我们上面那个CGI例子中的问题很熟悉。一个典型的Web程序员通常都会经过这样一些阶段。

1. 从零开始编写Web程序。
2. 从零开始编写另外一个Web程序。
3. 意识到第二个程序可以利用第一程序里面的很多东西。
4. 重构代码，在程序二中利用前一个程序的代码。
5. 重复2-4很多次。
6. 然后你认识到你以及发明了一个框架。

Django也是这样产生的！

Django是从真实的项目中成长起来的。它是由美国堪萨斯州（Kansas）Lawrence的个Web开发团队编写的。
它诞生于2003年秋天，当时 *Lawrence Journal-World* 的两个Web程序员开始用Python来编写程序。

他们这个World Online团队需要维护当地的几个新闻站点，最终得已在新闻界特有的快节奏开发环境中成长起来。
当时他们要维护的站点有：LJWorld.com， Lawrence.com以及KUsports.com，记者和管理层常常要求他们在
几天甚至几个小时之内增加新的功能。因此，Simon和Andrian不得不开发出一种能节约时间的Web框架来按时完成
这些任务。

2005年的夏天，这个框架已经被用来制作了绝大多数的World Online站点了。这时，Jocob Kaplan-Moss决定
以开源软件的形式发布者个框架。他们在2005年6月份发布了Django，这个名字来自于爵士吉他手Django Reinhardt。

经过这些年的发展，Django已经是一个拥有成千上万用户和贡献者的开源项目。Django的两个创始人Adrian和Jacob
仍然在为Django的发展掌握方向，但是Django的发展更多的是依靠社区团队的合作了。

这些历史都是有关联的，因为它们帮助解释了两个关键点：第一，Django最可爱的地方。Django诞生于新闻环境，
它提供了很多特性（比如 `第六章`_ 会说到的管理后台），非常适合内容类网站，如Amazon.com， craigslist.org
或者washingtonpost.com那样的提供动态的，数据库驱动的信息。不要看到这里就感到沮丧，Django特别擅长于动态
内容管理网站不等于说Django就是一个用来构建动态内容的网站。（在某些方面 *特别高效* 不等同于在其他方面 *不高效* ）

第二点是，Django的起源造就了它的开源社区文化。因为Django来自于真实世界的代码，而不是学术研究或者商业产品，
它真正专注于解决Web开发者们在开发中不断遇到的问题。这样，Django几乎每天都由进步，框架的维护者对确保Django能
够节约开发着的时间，编写处易于维护的高效的产品由恨大的兴趣。更重要的，
开发者也有很大的动力去解决他们自己的问题，去节省他们自己的时间。毕竟他们吃自己的狗粮(To put it bluntly, they eat their own dog food.)

.. AH The following sections are the type of content that typically appears
.. AH in a book's Introduction section, but we include it here because this
.. AH chapter serves as an introduction.

如何阅读本书
=====================

本书试图在可读性和参考性之间做一个平衡，当然会更偏向于可读性。如之前提到的，本书的目标是要让你成为
一个Django的专家，我们相信，要教会Django最好的方法就是提供足够的实例，而不是一堆详尽却乏味的Django
手册。（正如谚语说的：你不能指望光教字母就能教会人说话。）

我们推荐你按顺序阅读第1-12章，这些章节是如果使用Django的基础；读过这些内容后，你就可以用Django搭建网站了。
其中第1-7章是核心课程，第8-11章涵盖了Django的一些高级应用，第12章介绍部署的知识。剩下的13-20章，介绍Django的
其他特征，可以任意顺序阅读。

附录部分用作参考资料。你需要查看Django的部分功能或者需要查阅语法的时候，你可以回来翻阅这些资料，或者是 http://www.djangoproject.com/ 上的文档。 

需要的编程知识
------------------------------

本书的读者需要了解的面向过程或者面向对象编程的基础概念：流程控制(比如 ``if``, ``while``, ``for``)，
数据结构(列表，哈希表/字典)，变量，类和对象等。

当然，Web开发经验对阅读本书当时十分有用，不过并不是必须的。我们会尽量在本书中为缺乏经验的开发人员
介绍Web开发中的最佳实践。

需要的Python知识
-------------------------

本质上说，Django只是一个用Python编写的类库。要使用Django来开发网站，你需要编写Python代码
来使用这些类库。所以，学习Django其实就是学习如何用Python编程以及理解Django的运作方式。

如果你有过Python的编程经验，那么学习过程中不会遇到太多问题。基本上，Django的代码中并没有使用太多
“魔法”（比如难以理解的花哨的编程技巧）。对你来说，学习Django就是学习它的一个使用管理和API。

如果你没有使用Python编程的经验，你一定会学到很多东西。Python非常易学易用的！虽然本书并没有包含一个
完整的Python入门教程，但会在需要的地方强调用到的Python的特性。当然，我们还是推荐你阅读一下官方的Python
教程，访问 http://docs.python.org/tut/ 可以找到。另外我们也推荐Mark Pilgrim所写的 *Dive Into Python* ，
可以到 http://www.diveintopython.net/ 阅读。

Django的版本
-------------

本书针对Django 1.4.

Django的开发着尽量保证版本向后兼容，但是仍然可能会添加一些不向后兼容的改变。每个版本的更新通常都会
记录在relase note中，你可以到 https://docs.djangoproject.com/en/dev/releases/1.X 查看。


获得帮助
---------

Django的一个最大的优势就是她有一群乐于助人的人活跃在Django社区里。
你在Django中遇到的任何问题，从安装，程序设计，数据库设计到部署，都可以在上面寻求帮助。

* django-users邮件列表上面有上万的Django用户，活跃着讨论问题。可以到 http://www.djangoproject.com/r/django-users
  免费注册。

* 如果碰到棘手的问题，想要得到及时的回复，可以到Django IRC channel寻求帮助。加入方法是在Freenode IRC network上加入#django。

下一章
-------

`下一章`_, 我们将开始真正接触Django，将会教你安装和初始配置的步骤。

.. _下一章: chapter02.html
.. _第六章: chapter06.html
.. _第五章: chapter05.html
.. _CGI: http://zh.wikipedia.org/wiki/通用网关接口
