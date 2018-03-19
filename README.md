# postgres-plv8

Fork of [clkao/postgres-plv8](https://github.com/clkao/docker-postgres-plv8)


Docker images for running [plv8](https://github.com/plv8/plv8) based on Amazon RDS support (9.3, 9.4, 9.5, 9.6, 10) using the RDS supported plv8 version as defined in the [AWS Docs](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_PostgreSQL.html). Based on the [official Postgres image](http://registry.hub.docker.com/_/postgres/).

[![unidays/postgres-plv8][docker-pulls-image]][docker-hub-url] [![unidays/postgres-plv8][docker-stars-image]][docker-hub-url]

## Supported tags and respective `Dockerfile` links
- `9.5.2-1.4.4` ([9.5.2-1.4.4/Dockerfile](https://github.com/myunidays/docker-postgres-plv8/blob/master/9.5/Dockerfile))

## Usage

### How to use this image

This image behaves exactly like the official Postgres image with the only difference being the inclusion of the plv8 extension.

```sh
$ docker run --rm --name postgres -it unidays/postgres-plv8:9.5.2-1.4.4
$ docker run --rm --link postgres:postgres -it unidays/postgres-plv8:9.5.2-1.4.4 bash -c "psql -U postgres -h \$POSTGRES_PORT_5432_TCP_ADDR -t -c \"CREATE EXTENSION plv8; SELECT extversion FROM pg_extension WHERE extname = 'plv8';\""
```

You should see the version of the plv8 extension installed.

You can optionally create a service using `docker-compose`:

```yml
postgres:
  image: unidays/postgres-plv8:9.5.2-1.4.4
```

## Image variants

The `unidays/postgres-plv8` image comes in multiple flavors:

### `unidays/postgres-plv8:<postgresVersion>-<plv8Version>`

Points to the latest release available of Postgres `<postgresVersion>` on AWS RDS with the latest release available of plv8 `<plv8Version>` installed.

## Supported Docker versions

This image is officially supported on Docker version 1.10, with support for older versions provided on a best-effort basis.

## License

MIT

[docker-hub-url]: https://hub.docker.com/r/unidays/postgres-plv8/
[docker-pulls-image]: https://img.shields.io/docker/pulls/unidays/postgres-plv8.svg?style=flat-square
[docker-stars-image]: https://img.shields.io/docker/stars/unidays/postgres-plv8.svg?style=flat-square
