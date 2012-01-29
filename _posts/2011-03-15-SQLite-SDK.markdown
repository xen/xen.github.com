---
layout: post
title: "Проблемы с SQLite в SDK"
category: 
tags: ['issues']
---
Во время тестирования обнаружилась досадная проблема, оказывается при использовании SQLite в запросах которых присутствует ключ в качестве одного из критериев он игнорируется. Например такой код:

{% highlight python %}
q = Model.all()
q.filter("some_field", some_value)
q.filter("__key__ >", some_key) # Этот критерий будет проигнорирован
{% endhighlight %}

Причем если только ключ, то все будет найдено:

{% highlight python %}
q = Model.all()
q.filter("__key__ >", some_key) # Это сработает
{% endhighlight %}

Если переключиться на стандартное хранилище (а точнее не использовать флаг <code>--use_sqlite</code>), то будет работать как и ожидалось.

ID заявки <a href="http://code.google.com/p/googleappengine/issues/detail?id=3232">3232</a>, если у вас проблема тоже повторяется, то проголосуйте. 
