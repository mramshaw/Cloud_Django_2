# Cloud Django_2

[![Known Vulnerabilities](https://snyk.io/test/github/mramshaw/Cloud_Django_2/badge.svg?style=plastic&targetFile=requirements.txt)](https://snyk.io/test/github/mramshaw/Cloud_Django_2?style=plastic&targetFile=requirements.txt)

This project follows on from my [Writing_Django_2](https://github.com/mramshaw/Writing_Django_2) project, which is a simple Hello World in Django 2 LTS.

[As might be expected, there were breaking changes between Django and Django 2. So rather than upgrade my
 [Cloud_Django](https://github.com/mramshaw/Cloud_Django) repo, it seemed to be a better idea to create
 the __polls__ app in Django 2 and proceed from there, this time with Django 2 LTS.]

It will use [gunicorn](http://gunicorn.org/) which is a web server for [Django](https://docs.djangoproject.com/en/2.2/howto/deployment/wsgi/gunicorn/).
Specifically, it is a [WSGI](https://en.wikipedia.org/wiki/Web_Server_Gateway_Interface) server.

The plan of attack is as follows:

* [Install and test 'gunicorn'](#gunicorn)
* [Configure 'gunicorn'](#configure-gunicorn)

## gunicorn

To install locally:

    $ pip3 install --user gunicorn

Or simply use the `requirements.txt` file:

    $ pip3 install --user -r requirements.txt

Verify the version:

```bash
$ gunicorn --version
gunicorn (version 20.0.2)
$
```

Lets see if it runs (this needs to be in the same folder as `manage.py`):

```bash
$ cd polls
$ gunicorn polls.wsgi
[2019-11-27 09:49:15 -0500] [9766] [INFO] Starting gunicorn 20.0.2
[2019-11-27 09:49:15 -0500] [9766] [INFO] Listening at: http://127.0.0.1:8000 (9766)
[2019-11-27 09:49:15 -0500] [9766] [INFO] Using worker: sync
[2019-11-27 09:49:15 -0500] [9770] [INFO] Booting worker with pid: 9770
^C[2019-11-27 09:49:16 -0500] [9766] [INFO] Handling signal: int
[2019-11-27 14:49:17 +0000] [9770] [INFO] Worker exiting (pid: 9770)
[2019-11-27 09:49:17 -0500] [9766] [INFO] Shutting down: Master
$
```

## Configure gunicorn

Now we need to open up `gunicorn` with a `gunicorn.conf.py` file (for some reason,
this config file needs to be tagged as a Python file). By default, `gunicorn` runs
locally and will only accept local connections. We will configure it to run in
__promiscuous mode__ (which is a terrible practice, we should really run it behind
a front-end [__nginx__ is recommended], but we can fix this later).

We will create this file as follows:

```python3
import multiprocessing

bind = "0.0.0.0:8000"
workers = multiprocessing.cpu_count() * 2 - 1
```

Once more, with our config file:

```bash
$ gunicorn -c gunicorn.conf.py polls.wsgi
[2019-11-27 09:56:41 -0500] [9960] [INFO] Starting gunicorn 20.0.2
[2019-11-27 09:56:41 -0500] [9960] [INFO] Listening at: http://0.0.0.0:8000 (9960)
[2019-11-27 09:56:41 -0500] [9960] [INFO] Using worker: sync
[2019-11-27 09:56:41 -0500] [9965] [INFO] Booting worker with pid: 9965
[2019-11-27 09:56:41 -0500] [9967] [INFO] Booting worker with pid: 9967
[2019-11-27 09:56:41 -0500] [9969] [INFO] Booting worker with pid: 9969
[2019-11-27 09:56:41 -0500] [9970] [INFO] Booting worker with pid: 9970
[2019-11-27 09:56:41 -0500] [9973] [INFO] Booting worker with pid: 9973
[2019-11-27 09:56:41 -0500] [9975] [INFO] Booting worker with pid: 9975
[2019-11-27 09:56:41 -0500] [9976] [INFO] Booting worker with pid: 9976
^C[2019-11-27 09:57:14 -0500] [9960] [INFO] Handling signal: int
[2019-11-27 14:57:14 +0000] [9967] [INFO] Worker exiting (pid: 9967)
[2019-11-27 14:57:14 +0000] [9969] [INFO] Worker exiting (pid: 9969)
[2019-11-27 14:57:14 +0000] [9965] [INFO] Worker exiting (pid: 9965)
[2019-11-27 14:57:14 +0000] [9975] [INFO] Worker exiting (pid: 9975)
[2019-11-27 14:57:14 +0000] [9973] [INFO] Worker exiting (pid: 9973)
[2019-11-27 14:57:14 +0000] [9970] [INFO] Worker exiting (pid: 9970)
[2019-11-27 14:57:14 +0000] [9976] [INFO] Worker exiting (pid: 9976)
[2019-11-27 09:57:14 -0500] [9960] [INFO] Shutting down: Master
$
```

Okay, everything runs.

## To Do

- [x] Add a badge for `Black` formatting style
