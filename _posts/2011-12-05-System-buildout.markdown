---
layout: post
title: "Системный buildout"
category: 
tags: ['tips', 'buildout']
---
К стыду своему только на прошлой неделе додумался прочитать вывод `buildout --help`. Возможно этой команды раньше не было:

    init

       Initialize a buildout, creating a buildout.cfg file if it doesn't
       exist and then performing the same actions as for the buildout
       command.

В эта команда позволяет сэкономить несколько секунд времени при создании нового проекта. <!-- cut --> Для того чтобы воспользоваться всеми преимуществами прогресса надо с помощью системного `easy_install` или `pip` установить `zc.buildout` в качестве системной команды. Не буду вдаваться в детали этого процесса, поскольку он оказался несколько запутанным на моей машине со множеством разных версий python. Просто покажу вывод следующей команды:

{% highlight bash %}
$ ls -l `which buildout`
lrwxr-xr-x  1 root  staff    72B Nov 30 15:38 /usr/local/bin/buildout@ -> /opt/local/Library/Frameworks/Python.framework/Versions/2.7/bin/buildout
{% endhighlight %}

Теперь для того чтобы начать новый проект мне достаточно выполнить команду `buildout init`. Получается приблизительно так:

{% highlight bash %}
xen@Nekomusume.local  ~/Dev % mkdir proj 
xen@Nekomusume.local  ~/Dev % cd proj 
xen@Nekomusume.local  ~/Dev/proj % buildout init
Creating '/Users/xen/Dev/proj/buildout.cfg'.
Creating directory '/Users/xen/Dev/proj/bin'.
Creating directory '/Users/xen/Dev/proj/parts'.
Creating directory '/Users/xen/Dev/proj/eggs'.
Creating directory '/Users/xen/Dev/proj/develop-eggs'.
Generated script '/Users/xen/Dev/proj/bin/buildout'.
xen@Nekomusume.local  ~/Dev/proj % 
{% endhighlight %}

Структура проекта сгенерировалась автоматом, заодно создался конфигурационный файл с пустым содержимым.

{% highlight bash %}
xen@Nekomusume.local  ~/Dev/proj % ls -al 
total 8
drwxr-xr-x   7 xen  staff   238B Dec  5 13:35 ./
drwxr-xr-x@ 20 xen  staff   680B Dec  5 13:35 ../
drwxr-xr-x   3 xen  staff   102B Dec  5 13:35 bin/
-rw-r--r--   1 xen  staff    20B Dec  5 13:35 buildout.cfg
drwxr-xr-x   4 xen  staff   136B Dec  5 13:35 develop-eggs/
drwxr-xr-x   2 xen  staff    68B Dec  5 13:35 eggs/
drwxr-xr-x   3 xen  staff   102B Dec  5 13:35 parts/
xen@Nekomusume.local  ~/Dev/proj % cat buildout.cfg 
[buildout]
parts = 
{% endhighlight %}

И в папке `bin` сразу же появился исполняемый скрипт 

{% highlight bash %}
xen@Nekomusume.local  ~/Dev/proj % ls -al bin
total 8
drwxr-xr-x  3 xen  staff   102B Dec  5 13:35 ./
drwxr-xr-x  7 xen  staff   238B Dec  5 13:35 ../
-rwxr-xr-x  1 xen  staff   566B Dec  5 13:35 buildout*
xen@Nekomusume.local  ~/Dev/proj % 
{% endhighlight %}

Единственный нюанс: buildout привязан к python2.7 который является основной версией на моей машине. Но можно сделать нужные симлинки например под другими именами (например `buildout-2.6`) и вызывать их. 

Надеюсь кому-то этот совет сэкономит несколько секунд жизни.
