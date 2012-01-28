---
layout: post
title: "Серверная часть на... Javascript"
category: 
tags: ['code']
---
К тому, что в браузере нет выхода и приходится программировать только на одном языке уже можно привыкнуть. Некоторые обходят это ограничение путем использования Adobe Flash. Но недавно начался обратный процесс. Программисты начали говорить, что JavaScript является полноценным языком не только для выполнения в браузере. Конечно, основой для этого стала не тянущая рынок назад группа браузеров от Microsoft, а успех Mozilla Firefox и WebKit. 

Думаю эта новость будет приятной для тех кто видит будущее у отличного проекта <a href="http://commonjs.org/">CommonJS</a>. 

George Moschovitis представил анонс проекта <a href="http://www.appenginejs.org/">App Engine JavaScript SDK</a>. Вы все правильно поняли, теперь достаточно знать только JavaScript для создания серверных скриптов и запуска их под Google App Engine. Конечно не прямо сейчас, а тогда когда проект достигнет более стабильного состояния. 

Оценить код можно по примерам. Вот пример работы с базой данных:

<code>var db = require("google/appengine/ext/db");

var Category = db.Model("Category", {
    label: new db.StringProperty(),
    category: new db.ReferenceProperty({referenceClass: Category})
});

var c = new Category({keyName: "news", label: News"});
c.put();
var key = ...
var c1 = Category.get(key);
var c2 = Category.getByKeyName("news");
var categories = Category.all().limit(3).fetch();</code>

Или работа с картинками:

<code>var images = require("google/appengine/api/images");
var i = images.resize(params.image.data, 640, 480);</code>

Работает это все благодаря проекту <a href="http://www.mozilla.org/rhino/">Rhino</a>, реализации JavaScript на Java. Который запускается под виртуальной машиной. 

Домашняя страница проекта <a href="http://www.appenginejs.org/">www.appenginejs.org</a>, там же можно скачать пример проекта создающего блог и оценить готовность реализации поддержки разных API.
