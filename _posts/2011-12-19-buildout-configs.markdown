---
layout: post
title: "Дополнительные конфигурации в Buildout"
category: 
tags: ['tips', 'buildout']
---
Хочу добавить в копилку мелких удобств buildout возможность расширения конфигураций и создания альтернативных.<!-- cut -->

Мы столкнулись с проблемой создания двух параллельных конфигураций для Django проекта используя [djangorecipe](http://pypi.python.org/pypi/djangorecipe). Django уже имеет систему файлов настроек, но хочется не только регулировать параметры самого django, но и указывать какие именно пакеты устанавливать в окружение. Например `django-debug-toolbar` или `ipython` не нужны на сервере в рабочем режиме, но весьма удобны во время разработки.

Наверное уже обращали внимание на параметр `extends`, но одно дело использовать чьи-то готовые примеры, другое дело самому гибко настраивать. Используя эту директиву относительно удобно можно регулировать актуальный набор параметров конечного набора конфигурации. Например у вас есть начальный файл `buildout.cfg`:

    [buildout]
    parts =
      django
    
    eggs =
      ....
      site
      south
      django_markdown
        
    [django]
    recipe = djangorecipe
    settings = production
    project = site
    wsgi = true
    eggs =
      ${buildout:eggs}

Но для рабочего окружения нужен другой конфигурационный файл и дополнительные пакеты, поэтому создаем рядом файл `dev.cfg`:

    [buildout]
    extends = buildout.cfg
    eggs +=
      django-debug-toolbar
      ipython
    
    [django]
    settings = development

Вся магия происходит во второй строчке. В этом месте импортируется файл `buildout.cfg` и все остальное содержимое файла изменяет параметры по следующим правилам:

    # Добавить значения в список
    [section1]
    option += a3 a4

    # Убрать значения из списка
    [section2]
    option -= b1 b2

    # Назначить новые значения, если переменная или секция не была определенна,
    # то они будут созданы
    [section3]
    option = h1 h2 

Теперь можно указывать какой конфигурационный файл надо исполнить:

    $ bin/buildout -c dev.cfg

Ну и напоследок, во всем это можно очень быстро запутаться. Поэтому запомните ключ **annotate**:

    annotate

      Display annotated sections. All sections are displayed, sorted
      alphabetically. For each section, all key-value pairs are displayed,
      sorted alphabetically, along with the origin of the value (file name or
      COMPUTED_VALUE, DEFAULT_VALUE, COMMAND_LINE_VALUE).
 

Ссылки:

* [Adding and removing options](http://pypi.python.org/pypi/zc.buildout/1.5.2#adding-and-removing-options) в официальной документации по buildout.

