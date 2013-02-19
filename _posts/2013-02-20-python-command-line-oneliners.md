---
layout: post
title: "Python command line oneliners"
description: ""
category: 
tags: ['python', 'runglish', 'cli']
---
{% include JB/setup %}

There is list of pretty useful python small command line oneliners available out of the box. That means if you already have installed python on your system (most Linux, *BSD, including OSX) then you can use it even if you are not python developer.

I love most way to create fake SMTP server:

    $ python -m smtpd -n -c DebuggingServer localhost:25

If you develop using something else, but need to send emails and want to debug or *just need something* you can add to your `Makefile` this lines:

    mail:
        python -m smtpd -n -c DebuggingServer localhost:20025
        
And run `make mail`, voila you have running SMTP daemon on 20025 port. Easy!

**Make JSON beautiful**? Next oneliner:

    $ echo '{"foo": "lorem", "bar": "ipsum"}' | python -mjson.tool

You can only wish more if you like to see it in color. Hope you noticed pipe support. 

**Full set of available HTTP services**. 

    $ python -m SimpleHTTPServer
    $ python -m CGIHTTPServer

Wanna use different port?

    $ python -m SimpleHTTPServer 8080
    $ python -m CGIHTTPServer 9080

It serve all files from local folder and below.

**XMLRPC** is not very popular this days, but you still able to debug remote servers:

    $ python -c 'import xmlrpclib; print xmlrpclib.Server("http://host:8080").methodName(param,param2)'

Real life example? Search on PyPI:

    $ python -c 'import xmlrpclib; print xmlrpclib.Server("http://pypi.python.org/pypi").package_releases("Flask")'
    ['0.9']
    $ python -c 'import xmlrpclib; print xmlrpclib.Server("http://pypi.python.org/pypi").search({"name":"hopak"})'
    [{'_pypi_ordering': 12, 'version': '0.1.0', 'name': 'Flask-Hopak', 'summary': 'Admin interface for Flask that uses Hopak models'}, {'_pypi_ordering': 12, 'version': '0.4.2', 'name': 'hopak', 'summary': 'hopak framework base package'}]

More python specific **timeit** module, it allows to measure speed of small code snippets:

    $ python -m timeit '"-".join(str(n) for n in range(100))' 
    10000 loops, best of 3: 33.2 usec per loop
    $ python -m timeit '"-".join([str(n) for n in range(100)])'
    10000 loops, best of 3: 28.2 usec per loop

Even is possible to make a little setup using `-s` parameter:

    $ python -m timeit -s "import json" "json.dumps({'a':'a'})" 
    100000 loops, best of 3: 5.98 usec per loop
    
### More

Probably listed here is more common for me. But if you like to discover more, then simple scan standard python library for `if __name__ == '__main__'` substring and check results: 

    $ grep -r  "if __name__ == '__main__'" /path/to/python/libaray/
