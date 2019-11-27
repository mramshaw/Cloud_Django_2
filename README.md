# Cloud Django_2

[![Known Vulnerabilities](https://snyk.io/test/github/mramshaw/Cloud_Django_2/badge.svg?style=plastic&targetFile=requirements.txt)](https://snyk.io/test/github/mramshaw/Cloud_Django_2?style=plastic&targetFile=requirements.txt)

This project follows on from my [Writing_Django_2](https://github.com/mramshaw/Writing_Django_2) project, which is a simple Hello World in Django 2 LTS.

[As might be expected, there were breaking changes between Django and Django 2. So rather than upgrade my
 [Writing_Django](https://github.com/mramshaw/Writing_Django) repo, it seemd to be a better idea to create
 the __polls__ app in Django 2 and proceed from there, this time with Django 2 LTS.]

It will use [gunicorn](http://gunicorn.org/) which is a web server for [Django](https://docs.djangoproject.com/en/2.2/howto/deployment/wsgi/gunicorn/).
Specifically, it is a [WSGI](https://en.wikipedia.org/wiki/Web_Server_Gateway_Interface) server.

The plan of attack is as follows:

* [Install and test 'gunicorn'](https://github.com/mramshaw/Cloud_Django#gunicorn)

## gunicorn

To install locally:

    $ pip3 install --user gunicorn

Or simply use the `requirements.txt` file:

    $ pip3 install --user -r requirements.txt

Verify the version:

```bash
$ pip3 list --format=freeze | grep gunicorn
$
```

Lets see if it runs (this needs to be in the same folder as `manage.py`):

```bash
$ gunicorn polls.wsgi
$
```

Now we need to open up `gunicorn` with a `gunicorn.conf` file. By default, `gunicorn`
runs locally and will only accept local connections. We will configure it to run in
__promiscuous mode__ (which is a terrible practice, we should really run it behind a
front-end, but we can fix this later).

Once more, with a config file:

```bash
$ gunicorn -c ../gunicorn.conf polls.wsgi
$
```

Okay, everything runs.
