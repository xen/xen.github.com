---
layout: post
title: "Большой Flask проект"
description: "Руководство о том как сделать большой Flask проект"
category: 
tags: [flask, python, makefile]
---
{% include JB/setup %}

Так получилось, что я сейчас работаю по временному контракту на Google (это не вечно, поэтому даю ссылку на [мое резюме](http://careers.stackoverflow.com/xen)). 

Оказывается эта большая корпорация не только предоставляет рабочее место с пряничками, вкусно пахнущими смазками для всяких мест через которые вы можно подключиться к благоденствующему миру великой Матрицы и работать на благо победы SkyNet'а. Но и быть обычным гастрабайтером на контракте. Как вы понимаете так даже удобнее, не надо тратить время на утомительные собеседования, переезды в капиталистические страны, сталкиваться с ужасами оформления страховой медицины, подписывания документов и тд.

Моя работа заключается в создании некого инструмента, суть которого интересна не многим. Вы конечно можете его поставить и попробовать, но если вы не занимаетесь подготовкой шрифтов выпускаемых под открытой лицензией OFL и потом их не выкладываете в каталог <http://google.com/fonts/>, а таких людей ровно 1 во всем мире, то наверное инструмент не станет частью вашего рабочего окружения. 

Главное, что исходный код [Font Bakery](https://github.com/xen/fontbakery)  доступен под открытой лицензией Apache 2 и выложен на GitHub. Возможно я переоформлю в отдельный шаблон проекта в будущем, но пока некоторые важные части не представлены в проекте (например миграция и тесты).

<img src="/assets/img/font-bakery.png" />

> Хоть в чем-то мы оказывается совпадаем во вкусах с мегакорпорацией — и я и они верим, что все версии копилефт лицензии GPL это продукт рака мозга маразматика Столлмана и нельзя допускать существования этого отстоя в идеальном мире.

И еще одно отступление.

> Есть юридическая тонкость, хоть автором кода и являюсь я, но права и вообще все по контракту я передаю Google, а это значит, что это уже они выкладывают его под APL. Так это работает.

## Что внутри

Не имеет смысла углубляться суть проекта, он еще активно пилится. Не стоит его воспринимать как идеальный код, некоторые соглашения не нравятся мне, скорее всего они не покажутся красивыми вам. Главное что если вы столкнетесь с похожей задачей или вам непременно надо будет сделать что-то на Flask, то выкладки и код могут показаться полезными. Первый этап завершен, есть что показать, так что давайте расскажу о технологии. И проведу некоторый code review. 

Начну с пересечения сексуальных ключевых слов:

- С намеком на развесистое Flask приложение, не такое уж и большое, но сделано по правилам
- OAuth авторизация через GitHub
- Realtime сделанный на основе Socket.io
- Redis для очередей (на самом деле напрямую не задействован, а только как backend библиотеки rq)
- Gunicorn
- Gevent/greenlet

## Развертка окружения (Makefile, virtualenv, pip)

Одна из больших проблем, но мною лично очень любимая это развертка окружения проекта. Мне очень не нравятся всякие туториалы о том как надо протянуть левую пятку под правым плечом и нежно постучать себе по жопе для того чтобы поставить какой-то модуль. В идеальном мире проекты должны запускаться двумя командами **скачал** и **запустил**. К сожалению, этого достичь не возможно, особенно если у продукта подразумевается более чем одна инсталяция (более чем на одном типе окружения). 

Эта проблема вызывает возбуждение не только у меня, поэтому было создано большое множество инструментов, утилит и вообще вокруг этого существует целая индустрия (возможно даже где-то есть секретные братства и ордены). 

Сначало надо поставить себе задачу и потом ее решать. Поскольку проект преимущественно python, то прежде всего должно развернуться окружение. В python мире нет единого стандарта развертку окружения, как обычно портят воздух ущербные пользователи Windows у которых нет в стандартной поставке утилиты make. У нас она есть:

{% highlight makefile %}
# Makefile
venv/bin/activate:
    virtualenv-2.7 --system-site-packages venv

static/bootstrap/css/bootstrap.css:
    cd static && curl -O  http://getbootstrap.com/2.3.2/assets/bootstrap.zip && unzip bootstrap.zip

# target: setup — bootstrap environment
setup: venv/bin/activate requirements.txt static/bootstrap/css/bootstrap.css 
    . venv/bin/activate; pip install -Ur requirements.txt

# target: run — run project
run: venv/bin/activate requirements.txt
    . venv/bin/activate; python entry.py

# target: help — this help
help:
    @egrep "^# target:" [Mm]akefile

{% endhighlight %}

Полный файл лежит в репозитории. Если не знаком синтаксис Makefile, то обязательно прочтите руководство, в этом примере видно:

- Есть вспомогательный таргет `make help`, который парсит `[Mm]akefile` и выводит все строки начинающиеся с `# target:` удобно для встраивания подсказки
- Для цели `run` проверяется есть ли развернутое virtualenv, в этом просто убедиться проверив наличие файла `venv/bin/activate`, если нет то развернуть его. Так же для запуска требуется наличие скачанной статики. Это указано в параметрах реквизита команды `run`.
- `make setup` выведен отдельно, поскольку иногда требуется принудительное обновление окружения.
- Если есть развернутое виртуальное окружение, то в нем запускается локальный pip и ставит все зависимости из *requrements.txt*.
- Когда нужно запустить проект, то запускается локальный python.
- Поскольку проект не тестировался на других версиях python'а (а точнее есть некоторые зависимости от Си библиотек которых нет под 3.3) то явно вызывается virtualenv для 2.7.
- Обратите внимание: **в качестве отступов в Makefile используются только tab'ы**

> Правильно оформив `Makefile` можно автоматизировать всю работу по созданию полноценного рабочего окружения и пропадет необходимость бегать по компьютерам сотрудников и устанавливать нужные версии библиотек. Для ruby, javascript (node.js) проектов есть возможность так же разворачивать окружения.

## Структура и особенности конфигурирования Flask приложения

Листинг entry.py возьму из другого схожего приложения поскольку в этом есть некоторые особенности о которых будет рассказано ниже, но сейчас они будут мешать пониманию. Для удобства назовем его bigapp:

{% highlight python %}
# entry.py
from bigapp import create_app, init_app

app = create_app(app_name='bigapp')
app.config.from_object('config')
app.config.from_pyfile('local.cfg', silent=True)
init_app(app)

if __name__ == '__main__':
    import os
    app.config['DEBUG'] = True
    app.run()

{% endhighlight %}

Весь модуль bigapp отдает на экспорт две функции [`create_app`](https://github.com/xen/fontbakery/blob/afafbc77a3575a00d3861882e199c4b896b9aed3/bakery/app.py#L35) и [`init_app`](https://github.com/xen/fontbakery/blob/afafbc77a3575a00d3861882e199c4b896b9aed3/bakery/app.py#L42). Так сделано по очень простой причине между вызовом `create_app` и `init_app` находится попытка прочитать конфигурацию приложения, причем дважды. Первый раз файл [`config.py`](https://github.com/xen/fontbakery/blob/afafbc77a3575a00d3861882e199c4b896b9aed3/config.py), а второй раз попытаться прочитать `local.cfg`, но если он не будет найден, то просто промолчать. Объясню почему так сдлеано:

- Понятно, что конфигурацию удобно хранить не внутри кода, а в специально отведенном для этого месте. 
- Понятно, что локальная конфигурация разработчика будет сильно отличаться от конфирурации production сервера, а она в свою очередь от окружения тестового сервера.
- Переменные с адресом подключения к базе данных, логины и пароли почтового сервера, id facebook или vk.com приложений будут не тестовые, а реальные. 

Поэтому и разнесены эти два файла, в первом хранится вообще вся конфигурация, во второром (а он не нужен на компе разарботчика) то что туда запишет системный администратор который будет делать релиз и выкладку проекта на реальный хостинг. Кроме того, обратите внимание на `.gitignore`, в нем [есть исключение](https://github.com/xen/fontbakery/blob/afafbc77a3575a00d3861882e199c4b896b9aed3/.gitignore#L38) на `local.cfg`. Это может создать проблемы только если вы используете выкладку на Heroku, потому что в этом случае вам надо будет добавлять реальный `local.cfg` в репозиторий. 

> Если бы подразумевалось, что Font Bakery нужно выкладывать на Heroku, то вместо *local.cfg* данные бы считывались из переменных окружения, но это не является частью данной статьи. Пока проект никуда не выкладывается.

После того как произойдет попытка собрать окончательную версию конфигурации происходит инициализация всех частей проекта. Это видно в функции [`init_app`](https://github.com/xen/fontbakery/blob/afafbc77a3575a00d3861882e199c4b896b9aed3/bakery/app.py#L42). 

С моей точки зрения некрасиво получается регистрация глобальных переменных в [gvars](https://github.com/xen/fontbakery/blob/afafbc77a3575a00d3861882e199c4b896b9aed3/bakery/app.py#L88), но такова архитектура Flask и таких мест пока в масштабах проекта пока мало. Не знаю будет ли их существенно больше на очень крупных проектах:

{% highlight python %}
def gvars(app):
    # Place to register project-wide global variables.
    from gitauth.models import User

    @app.before_request
    def guser():
        g.user = None
        if 'user_id' in session:
            if session['user_id']:
                #pylint:disable-msg=E1101
                user = User.query.get(session['user_id'])
                if user:
                    g.user = user
                else:
                    del session['user_id']
{% endhighlight %}

В строке `user = User.query.get(session['user_id'])` происходит обращение к базе данных, да при каждом запросе. В совсем крупном проекте надо было бы выделить сервер сессий и класть сессии туда.

## Расширения

Каждое расширение сначала просто создает экземпляр, потому что в момент создания расширения не известно какая у него будет конфигурация и не создан экзмепляр Flask приложения. В данном примере *github* выглядит совсем не красиво, ведь он даже не хранит свои настройки в общей конфигурации. К сожалению на это пришлось пойти чтобы не писать своего wrapper'а вокруг библиотеки rauth. 

> [Подробнее о том как использовать Flask, raurh и GitHub](http://www.vurt.ru/eng/2013/06/using-flask-and-rauth-for-github-authentication/)

{% highlight python %}
# extensions.py
from flask.ext.sqlalchemy import SQLAlchemy
db = SQLAlchemy()

from flask.ext.mail import Mail
mail = Mail()

from rauth.service import OAuth2Service

# Because of restrictions of OAuth2Service this lines can't be moved to config.py
GITHUB_CLIENT_ID = '4a1a8295dacab483f1b5'
GITHUB_CLIENT_SECRET = 'ec494ff274b5a5c7b0cb7563870e4a32874d93a6'

github = OAuth2Service(
    name='github',
    base_url='https://api.github.com/',
    access_token_url='https://github.com/login/oauth/access_token',
    authorize_url='https://github.com/login/oauth/authorize',
    client_id= GITHUB_CLIENT_ID,
    client_secret= GITHUB_CLIENT_SECRET,
)

from flask_flatpages import FlatPages
pages = FlatPages()

from flask.ext.rq import RQ
rq = RQ()
{% endhighlight %}

Потом каждому расширению в файле [`bakery/app.py:60`](https://github.com/xen/fontbakery/blob/afafbc77a3575a00d3861882e199c4b896b9aed3/bakery/app.py#L57) через метод `.init_app()` передается экземпляр сконфирурированного Flask приложения из которого наконец-то можно считать конечные настройки и зарегистрировать свои:

{% highlight python %}
def extensions_fabrics(app):
    db.init_app(app)
    mail.init_app(app)
    babel = Babel(app)
    pages.init_app(app)
    rq.init_app(app)

{% endhighlight %}

## Чертежи (Flask blueprint)

Наконец-то, чтобы закончить с конфигурацией подошли к модулям, это еще один способ изменить работу Flask'а. Если расширения меняют возможности (можно воспринимать как регистрацияю новых сервисов), то blueprint'ы ближе к добавлению новых подразделов сайта.

Собственно весь код отвечающий за отображаемые страниц находится в blueprint'ах. В текущий момент у проектах их несколько, разделение не совсем удачное, но это то что есть на текущий момент:

- gitauth - страницы авторизации, расположены за `/auth/`
- frontend - главная страница, страницы документации и в будущем возможно страницы результатов поиска
- realtime - страницы взаимодействия в режиме реального времени через *socket.io*
- settings — страницы персональных настроек пользователя
- project — страницы проектов

Устройство Flask blueprint достаточно простое, оно напоминает обычное Flask приложение за той лишь разницей, что маршрутизация (routing) строится относительно самого bluerprint'а, а не приложения. Для упрощения понимания стоит думать как буд-то бы каждый blueprint может быть подключен к одному и тому же сайту несколько раз (что является правдой).

Весь код выглядит приблизительно так:

{% highlight python %}
from flask import (Blueprint, render_template, Response, g, request,
    current_app, send_from_directory)

from ..project.models import Project

# создание самого модуля
frontend = Blueprint('frontend', __name__)
# если дописать параметр url_prefix, то вся маршрутизация будет 
# строиться относительно префикса
# frontend = Blueprint('frontend', __name__, url_prefix='/test')

@frontend.before_request
def before_request():
    # обычный хук который выполнится при обращении к любой странице этого модуля
    if g.user:
        g.projects = Project.query.filter_by(login=g.user.login).all()

@frontend.route('/')
def splash():
    # Главная страница, проверяет если пользователь залогинен, то 
    # показывает его проекты, если нет, то обычную главную страниу
    # Splash page, if user is logged in then show dashboard
    if g.user is None:
        return render_template('splash.html')
    else:
        projects = Project.query.filter_by(login=g.user.login).all()
        return render_template('dashboard.html', repos = projects)
{% endhighlight %}

Чтобы зарегистрировать все модули используется простой код:

{% highlight python %}
# blueprints
from .gitauth import gitauth
from .frontend import frontend
from .realtime import realtime
from .api import api
from .settings import settings
from .project import project

def init_app(app):
    # Register all blueprints and init extensions.
    app.register_blueprint(gitauth)
    app.register_blueprint(frontend)
    app.register_blueprint(realtime)
    app.register_blueprint(settings)
    app.register_blueprint(api)
    # keep it last
    app.register_blueprint(project)

{% endhighlight %}

## На что обратить внимание

Для удобства все модели определенные внутри модулей доступны через главный модуль `models.py`. Поскольку каждая модель отображается на свою отдельную таблицу, но в той же самой базе данных, то не стоит переживать, что названия могут повторяться, внутри базы данных названия таблиц тоже должны быть уникальны.

Декоратор [`login_required`](https://github.com/xen/fontbakery/blob/afafbc77a3575a00d3861882e199c4b896b9aed3/bakery/decorators.py#L22) позволяет защитить страниц от неавторизированного доступа:

{% highlight python %}
from functools import wraps
from flask import g, request, redirect, url_for, flash
from flask.ext.babel import gettext as _

def login_required(f):
    """ Decorator allow to access route only logged in user. Usage:

        @app.route('/test', methods=['GET'])
        @login_required
        def test():
            return "You are logged in"

    """
    @wraps(f)
    def decorated_function(*args, **kwargs):
        if g.user is None:
            flash(_('Login required'))
            return redirect(url_for('gitauth.login', next=request.url))
        return f(*args, **kwargs)
    return decorated_function
{% endhighlight %}

Если пользователь залогинен на GitHub'е и уже дал права Font Bakery, то при попытке зайти на страницу из примера к декторатору `/test` 

- Его перебросит на страницу `/auth/login?next=/test` (`url_for('gitauth.login', next=request.url)`)
- Эта страница перебросит его на GitHub страницу авторизации
- GitHub увидит, что пользователь уже дал права, выдаст пользователю код для обмена на токен
- Перебросит на `/auth/callback?next=/test&code=XXX`
- Вид [`/callback`](https://github.com/xen/fontbakery/blob/master/bakery/gitauth/views.py#L62) попытается обменять *code* на токен и если получится, то перебросит пользователя на страницу в переменной *next*

Для пользователя это будет выглядеть как секундная задержка перед заходом на закрытую страницу. 


## Продолжение следует

Если будет достаточный интерес к этому посту и дойдут руки, то в следующем посте я постараюсь подробнее расписать как работает Flask с вебсокетами, *socket.io* и фоновыми задачами.


