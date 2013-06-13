---
layout: post
title: "SQLite auto increment in Flask SQLAlchemy"
description: ""
category: 
tags: ["flask", "sqlite", "eng", "bakey", "sqlalchemy"]
---
{% include JB/setup %}

# Sqlite primary key

Working with [Bakery](https://github.com/xen/bakery/) I noticed strange behaviour of Sqlite. One of my models looks like (this is Flask-SQLAlchemy notation):

{% highlight python %}
from ..extensions import db

class Project(db.Model):
    __tablename__ = 'project'
    id = db.Column(db.Integer, primary_key=True)
    prop = db.Column(db.String(60), index=True)
    # other fields
{% endhighlight %}

I thought that if field is `primary_key` then it is auto increment. Generally it is true, since all new items that I have added to database have next `id`. Problem came when I have deleted some data and added new. Surprisingly next item use next number after max current used `id`. It was a little confusing and doesn't fit my needs. 

So I made this changes to model:

{% highlight python %}
from ..extensions import db

class Project(db.Model):
    __tablename__ = 'project'
    __table_args__ = {'sqlite_autoincrement': True}
    id = db.Column(db.Integer, primary_key=True)
    prop = db.Column(db.String(60), index=True)
    # other fields
{% endhighlight %}

And re-init all data, this solves my problem:

{% highlight sql %}
$ sqlite3 data.old.sqlite
sqlite> .schema project 
CREATE TABLE project (
	id INTEGER NOT NULL, 
	prop VARCHAR(60),
	PRIMARY KEY (id)
);
CREATE INDEX ix_project_prop ON project (prop);

$ sqlite3 data.sqlite
sqlite> .schema project 
CREATE TABLE project (
	id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
	prop VARCHAR(60),
	PRIMARY KEY (id)
);
CREATE INDEX ix_project_prop ON project (prop);
{% endhighlight %}
