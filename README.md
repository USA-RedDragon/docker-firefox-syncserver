<p align="center"><a href="https://github.com/USA-RedDragon/docker-firefox-syncserver" target="_blank"><img height="128" src="https://raw.githubusercontent.com/USA-RedDragon/docker-firefox-syncserver/master/.res/docker-firefox-syncserver.jpg"></a></p>

<p align="center">
  <a href="https://hub.docker.com/r/jamcswain/firefox-syncserver/tags?page=1&ordering=last_updated"><img src="https://img.shields.io/github/v/tag/USA-RedDragon/docker-firefox-syncserver?label=version&style=flat-square" alt="Latest Version"></a>
  <a href="https://github.com/USA-RedDragon/docker-firefox-syncserver/actions?workflow=build"><img src="https://img.shields.io/github/workflow/status/USA-RedDragon/docker-firefox-syncserver/build?label=build&logo=github&style=flat-square" alt="Build Status"></a>
  <a href="https://hub.docker.com/r/jamcswain/firefox-syncserver/"><img src="https://img.shields.io/docker/stars/jamcswain/firefox-syncserver.svg?style=flat-square&logo=docker" alt="Docker Stars"></a>
  <a href="https://hub.docker.com/r/jamcswain/firefox-syncserver/"><img src="https://img.shields.io/docker/pulls/jamcswain/firefox-syncserver.svg?style=flat-square&logo=docker" alt="Docker Pulls"></a>
</p>

## About

🐳 [Firefox Sync Server](http://moz-services-docs.readthedocs.io/en/latest/howtos/run-sync-1.5.html) Docker image based on Python Alpine Linux.<br />
If you are interested, [check out](https://hub.docker.com/r/jamcswain/) my other Docker images!

## Features

* Run as non-root user
* Multi-platform image
* [Traefik](https://github.com/containous/traefik-library-image) as reverse proxy and creation/renewal of Let's Encrypt certificates (see [this template](examples/traefik))

## Docker

### Tags

This image offers support for multiple SQL drivers in the form of tag suffixes.
You can append these to any version tag or `latest` in order to use the variant
with your preferred SQL driver installed. For example, to use PostgreSQL, you'd likely
want `jamcswain/firefox-syncserver:latest-postgresql`, or `jamcswain/firefox-syncserver:postgresql`.

#### Notable tags

* `latest` has MySQL support by default and is the same as `mysql`
* `edge` has MySQL support by default, potentially untested beta versions
* `mysql`: the same as `latest`
* `postgresql`: the same as `latest-postgresql`.
* `sqlite`: the same as `latest-sqlite`.

#### Tag suffixes

* `-mysql`: Enables MySQL support
* `-postgresql`: Enables PostgreSQL support
* `-sqlite`: Enables SQLite support

### Multi-platform image

Following platforms for this image are available:

```
$ docker run --rm mplatform/mquery jamcswain/firefox-syncserver:latest
Image: jamcswain/firefox-syncserver:latest
 * Manifest List: Yes
 * Supported platforms:
   - linux/amd64
   - linux/arm/v6
   - linux/arm/v7
   - linux/arm64
   - linux/386
   - linux/ppc64le
   - linux/s390x
```

### Notable Build Arguments

* `FF_SYNCSERVER_SQL_DRIVER`: Either `mysql`, `postgresql`, or `sqlite` (default).

### Environment variables

* `TZ`: The timezone assigned to the container (default `UTC`)
* `PUID`: Process UID (default `1000`)
* `PGID`: Process GID (default `1000`)
* `FF_SYNCSERVER_ACCESSLOG`: Display access log (default `false`)
* `FF_SYNCSERVER_LOGLEVEL`: Log level output (default `info`)
* `FF_SYNCSERVER_PUBLIC_URL`: Must be edited to point to the public URL of your server (default `http://localhost:5000`).
* `FF_SYNCSERVER_SECRET`: This is a secret key used for signing authentication tokens. It should be long and randomly-generated.
* `FF_SYNCSERVER_ALLOW_NEW_USERS`: Set this to `false` to disable new-user signups on the server. Only request by existing accounts will be honoured (default `true`).
* `FF_SYNCSERVER_FORCE_WSGI_ENVIRON`: Set this to `true` to work around a mismatch between public_url and the application URL as seen by python, which can happen in certain reverse-proxy hosting setups (default `false`).
* `FF_SYNCSERVER_SQLURI`: Defines the database in which to store all server data (default `sqlite:///data/syncserver.db`).
* `FF_SYNCSERVER_FORWARDED_ALLOW_IPS`: Set this to `*` or an IP range if you use an Nginx reverse proxy (optional).

> 💡 `FF_SYNCSERVER_SECRET_FILE` can be used to fill in the value from a file, especially for Docker's secrets feature.

### Volumes

* `/data`: Contains SQLite database if `FF_SYNCSERVER_SQLURI` is untouched

> :warning: Note that the volumes should be owned by the user/group with the specified `PUID` and `PGID`. If you don't give the volume correct permissions, the container may not start.

### Ports

* `5000`: Gunicorn port

## Use this image

### Docker Compose

Docker compose is the recommended way to run this image. You can use the following [docker compose template](examples/compose/docker-compose.yml), then run the container:

```bash
docker-compose up -d
docker-compose logs -f
```

### Command line

You can also use the following minimal command:

```bash
$ docker run -d -p 5000:5000 --name firefox_syncserver \
  -e TZ="Europe/Paris" \
  -e FF_SYNCSERVER_SECRET="5up3rS3kr1t" \
  -v $(pwd)/data:/data \
  jamcswain/firefox-syncserver:latest
```

## Notes

### Use with MySQL database

Set `FF_SYNCSERVER_SQLURI=pymysql://user:password@mysql_server_ip/db_name`

## Upgrade

Recreate the container whenever I push an update:

```bash
docker-compose pull
docker-compose up -d
```

## How can I help

All kinds of contributions are welcome :raised_hands:! The most basic way to show your support is to star :star2: the project, or to raise issues :speech_balloon: You can also support this project by [**becoming a sponsor on GitHub**](https://github.com/sponsors/USA-RedDragon) :clap: or by making a [Paypal donation](https://www.paypal.me/crazyws) to ensure this journey continues indefinitely! :rocket:

Thanks again for your support, it is much appreciated! :pray:

## License

MIT. See `LICENSE` for more details.
