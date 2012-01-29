---
layout: post
title: "Обновление bootstrap.py"
category: 
tags: ['dev']
---
Сегодня мы столкнулись с небольшой проблемой. Проект отказался разворачивать рабочее окружение стандартными командами:

{% highlight bash %}
$ python2.5 bootstrap.py --distribute
$ bin/buildout
{% endhighlight %}

Мы начали получать сообщение:

{% highlight pytb %}
Traceback (most recent call last):
  File "bin/buildout", line 22, in &lt;module&gt;
    import zc.buildout.buildout
ImportError: No module named zc.buildout.buildout
{% endhighlight %}

Оказалось проблема в том, что ребята из Zope Corporation обновили версии своих продуктов. Для того чтобы исправить проблему надо либо перейти на следующую версию zc.buildout, либо прямо указывать версию:

{% highlight python %}
$ python bootstrap.py --distribute --version=1.4.4
{% endhighlight %}

Переход не займет много времени, просто замените старый bootstrap.py на новый <a href="http://svn.zope.org/repos/main/zc.buildout/branches/1.4/bootstrap/bootstrap.py">http://svn.zope.org/repos/main/zc.buildout/branches/1.4/bootstrap/bootstrap.py</a>

Более подробная информация доступна в рассылке distutils <a href="http://mail.python.org/pipermail/distutils-sig/2010-August/016745.html">http://mail.python.org/pipermail/distutils-sig/2010-August/016745.html</a>
