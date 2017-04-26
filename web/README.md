Mailman 3 Web UI
================

This image consists of Mailman3's Web UI(Postorius) and Archiver
(Hyperkitty). This image is built from latest sources on [gitlab][1]. In future,
latest and stable releases will be seperate. I am looking forward to the release
of Mailman Suite 3.1 before that.

Configuration
=============

These are the settings that you MUST change before deploying:

- `SERVE_FROM_DOMAIN`: The domain name from which Django will be served. To be
  added to `ALLOWED_HOSTS` in django settings. Default value is not set.

- `HYPERKITTY_API_KEY`: Hyperkitty's API Key, should be set to the same value as
  set for the mailman-core.

These are the settings that are set to sane default and you do not need to
change them unless you know what you want.

- `DATABASE_URL`: URL of the type
  `driver://user:password@hostname:port/databasename` for the django to use. If
  not set, the default is set to
  `sqlite:///opt/mailman-web-data/mailmanweb.db`. The standard
  docker-compose.yaml comes with it set to a postgres database. It is not must
  to change this if you are happy with postgresql.

- `MAILMAN_REST_URL`: The URL to the Mailman core's REST API server.  Defaut
  value is `http://mailman-core:8001`.

- `MAILMAN_REST_USER`: Mailman's REST API username. Default value is `restadmin`

- `MAILMAN_REST_PASSWORD`: Mailman's REST API user's password. Default value is
  `restpass`

- `MAILMAN_HOST_IP`: IP of the Container from which Mailman will send emails to
  hyperkitty (django). Set to `172.19.199.2` by default.

- `SMTP_HOST`: IP Address/hostname from which you will be sending
  emails. Default value is `172.19.199.1`, which is the address of the Host OS.

- `SMTP_PORT`: Port used for SMTP. Default is `25`.

- `DJANGO_LOG_URL`: Path to the django's log file. Defaults to
  `/opt/mailman-web-data/logs/mailmanweb.log`.

Running
=======

It is highly recommended that you run this using the [docker-compose.yaml][2]
provided in the [github repo][3] of this project. You will need to proxy the
requests the container that you create with this image using an actual web
server like Nginx. The [github repo][3] provides the setup instructions for
Nginx.

Since the setup has `USE_SSL` set to `True` in django's `settings.py`, you may
also want to get a SSL certificate if you don't already have one. [Lets
Encrypt][4] provides free SSL certiticates for everyone and there are _some_
instructions about that also.

After the first run, you can create a superuser for django using the following
command:

```bash
    $ docker exec -it mailman-web python manage.py createsuperuser
```

[1]: https://gitlab.com/mailman
[3]: https://github.com/maxking/docker-mailman/
[2]: https://github.com/maxking/docker-mailman/blob/master/docker-compose.yaml
[4]: https://letsencrypt.org