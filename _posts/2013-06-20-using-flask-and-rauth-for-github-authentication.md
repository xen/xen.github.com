---
layout: post
title: "Using Flask and rauth for Github authentication"
description: "Real world example"
category: 'Eng'
tags: ['flask', 'rauth', 'eng']
---
{% include JB/setup %}

Recently I made a research about OAuth libraries for Flask. Picture is not bright, most packages are outdated and abandoned. Well known Flask-Oauth is generally dead and I don't recommend to rely on it. Funny that OAuth itself is very simple, after some magic during authentication you have `access_token` which you only need to add as GET parameter or special header to your request. Probably other providers need more complex logic but it is enough for Github. 

Trying to [fix Flask-OAuth I asked question on SO](http://stackoverflow.com/q/15964268/85739) and got answer from author of `rauth`, he recommend to use his library. And this post is my way to payback to him and open source. I made this real life example of how you can use `Flask`, `rauth`, Github and `sessions` to authorize users on your site.

First of all you need to setup your local environment. Make sure that you have installed `python`, `pip` and `virtualenv`. Then setup environment:

	$ virtualenv venv
	$ source venv/bin/activate 
	(venv)$ pip install rauth Flask Flask-SQLAlchemy

If you only want to run my example then you also need Sqlite python binding, which required by SQLAlchemy:

	(venv)$ pip install pysqlite

`pip` install all dependencies, so you should have everything ready.

Now you need to go to [*Applications* sections](https://github.com/settings/applications) in your Github settings area and setup new your own. This is example of what I have in my area:

![image](/assets/img/githubappsetup.png)

Time for code. This is my main module:

{% highlight python %}
# github.py
from flask import (Flask, flash, request, redirect, 
	render_template, url_for, session)
from flask.ext.sqlalchemy import SQLAlchemy

from rauth.service import OAuth2Service

# Flask config
SQLALCHEMY_DATABASE_URI = 'sqlite:///github.db'
SECRET_KEY = '\xfb\x12\xdf\xa1@i\xd6>V\xc0\xbb\x8fp\x16#Z\x0b\x81\xeb\x16'
DEBUG = True

# Flask setup
app = Flask(__name__)
app.config.from_object(__name__)
db = SQLAlchemy(app)

# Use your own values in your real application  
github = OAuth2Service(
    name='github',
    base_url='https://api.github.com/',
    access_token_url='https://github.com/login/oauth/access_token',
    authorize_url='https://github.com/login/oauth/authorize',
    client_id= '477151a6a9a9a25853de',
    client_secret= '23b97cc6de3bea712fddbef70a5f5780517449e4',
)

# models
class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    login = db.Column(db.String(80), unique=True)
    name = db.Column(db.String(120))

    def __init__(self, login, name):
        self.login = login
        self.name = name

    def __repr__(self):
        return '<User %r>' % self.login

    @staticmethod
    def get_or_create(login, name):
        user = User.query.filter_by(login=login).first()
        if user is None:
            user = User(login, name)
            db.session.add(user)
            db.session.commit()
        return user

# views
@app.route('/')
def index():
    return render_template('login.html')

@app.route('/about')
def about():
    if session.has_key('token'):
        auth = github.get_session(token = session['token'])
        resp = auth.get('/user')
        if resp.status_code == 200:
            user = resp.json()

        return render_template('about.html', user = user)
    else:
        return redirect(url_for('login'))


@app.route('/login')
def login():
    redirect_uri = url_for('authorized', next=request.args.get('next') or 
    	request.referrer or None, _external=True)
    print(redirect_uri)
    # More scopes http://developer.github.com/v3/oauth/#scopes
    params = {'redirect_uri': redirect_uri, 'scope': 'user:email'} 
    print(github.get_authorize_url(**params))
    return redirect(github.get_authorize_url(**params))

# same path as on application settings page
@app.route('/github/callback')
def authorized():
    # check to make sure the user authorized the request
    if not 'code' in request.args:
        flash('You did not authorize the request')
        return redirect(url_for('index'))

    # make a request for the access token credentials using code
    redirect_uri = url_for('authorized', _external=True)

    data = dict(code=request.args['code'],
        redirect_uri=redirect_uri,
        scope='user:email,public_repo')

    auth = github.get_auth_session(data=data)

    # the "me" response
    me = auth.get('user').json()

    user = User.get_or_create(me['login'], me['name'])

    session['token'] = auth.access_token
    session['user_id'] = user.id

    flash('Logged in as ' + me['name'])
    return redirect(url_for('index'))

if __name__ == '__main__':
    db.create_all()
    app.run()
{% endhighlight %}

And templates:

{% highlight html %}
<!-- templates/about.html -->
<!doctype html>
<title>Login</title>
{% for message in get_flashed_messages() %}
{{ message }}
{% endfor %}
<h1>About you</h1>
Welcome {{ user.name }}, and your email is {{ user.email }}
<br />
<a href ="{{ url_for('index') }}">Main page</a>
{% endhighlight %}

{% highlight html %}
<!-- templates/login.html -->
<!doctype html>
<title>Login</title>
{% for message in get_flashed_messages() %}
{{ message }}
{% endfor %}
<h1>Login</h1>
<a href ="{{ url_for('login') }}">Login with OAuth2</a>
<h1>About</h1>
<a href="{{ url_for('about') }}">About</a> If you are not logged in then you will be redirected to login page.
{% endhighlight %}

Database will be created after start:

	(venv)$ python github.py

After start open site in browser and login. `access_token` saved in session, it is not secure, but enough for example. In real site you can use something like this:

{% highlight python %}
# Probably your User model placed in different blueprint
from authmodule.models import User

@app.before_request
def before_request():
    g.user = None
    if 'user_id' in session:
        if session['user_id']:
            user = User.query.get(session['user_id'])
            if user:
                g.user = user
            else:
                del session['user_id']
{% endhighlight %}

Links:

- `rauth` <https://github.com/litl/rauth>
- Github developers API <http://developer.github.com/>
- `Flask` <http://flask.pocoo.org/>
- `SQLAlchemy` <http://www.sqlalchemy.org/>
- `Flask-SQLAlchemy` <http://pythonhosted.org/Flask-SQLAlchemy/>
- More complex applicaion example `bakery` <https://github.com/xen/bakery/>
