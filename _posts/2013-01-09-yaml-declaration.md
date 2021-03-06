---
layout: post
title: "Какой способ определения модели нравится больше? "
description: ""
category: 
tags: [hopak, yaml, metaclass]
---
{% include JB/setup %}

Продолжаю проектировать модели для [hopak](https://github.com/xen/hopak) и столкнулся со следующей дилеммой в дизайне. Сейчас модели определяются путем парсинга специально подготовленного `.yaml` файла и таким образом строится класс модели. Подразумевается, что гибридный способ определения модели является законным. Но если кому-то не нравится писать сначала `.yaml`, а потом еще и python файлы, то это тоже должно быть разрешено ([тикет](https://github.com/xen/hopak/issues/4)). 

Собственно и возникает вопрос, как более python-way и с точки зрения логики лучше декларировать что YAML определение не нужно. Вот два варианта:

<script src="https://gist.github.com/4493562.js"></script>

Мне больше нравится первый вариант, он более явный (явное лучше скрытого) и хоть и дает чуть меньше автоматизма.

В качестве бонуса сейчас работает такой вариант, он максимально лаконичный, но заставляет программиста думать о том что где-то спрятано что-то еще.

<script src="https://gist.github.com/4493683.js"></script>