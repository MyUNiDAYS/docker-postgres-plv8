# postgres-plv8

Inspired by of [clkao/postgres-plv8](https://github.com/clkao/docker-postgres-plv8)

Located on [ECR Public][ecr-hub-url].


Docker images for running [plv8](https://github.com/plv8/plv8) based on Amazon RDS support (9.3, 9.4, 9.5, 9.6, 10) using the RDS supported plv8 version as defined in the [AWS Docs](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_PostgreSQL.html). Based on the [official Postgres image](http://registry.hub.docker.com/_/postgres/).

## Supported tags and respective `Dockerfile` links
- `9.5.12-2.1.0` ([9.5.12-2.1.0/Dockerfile](https://github.com/myunidays/docker-postgres-plv8/blob/master/9.5.12/2.1.0/Dockerfile))
- `9.6.8-2.1.0` ([9.6.8-2.1.0/Dockerfile](https://github.com/myunidays/docker-postgres-plv8/blob/master/9.6.8/2.1.0/Dockerfile))
- `13.2-2.3.15` ([13.2-2.3.15/Dockerfile](https://github.com/myunidays/docker-postgres-plv8/blob/master/13.2-2.3.15/Dockerfile))
- `13.3-2.3.15` ([13.3-2.3.15/Dockerfile](https://github.com/myunidays/docker-postgres-plv8/blob/master/13.3-2.3.15/Dockerfile))

## Usage

### How to use this image

This image behaves exactly like the official Postgres image with the only difference being the inclusion of the plv8 extension.

```sh
$ docker run --rm --name postgres -it public.ecr.aws/f3w3g2g6/docker-postgres-plv8:13.2-2.3.15
$ docker run --rm --link postgres:postgres -it public.ecr.aws/f3w3g2g6/docker-postgres-plv8:13.2-2.3.15 bash -c "psql -U postgres -h \$POSTGRES_PORT_5432_TCP_ADDR -t -c \"CREATE EXTENSION plv8; SELECT extversion FROM pg_extension WHERE extname = 'plv8';\""
```

You should see the version of the plv8 extension installed.

You can optionally create a service using `docker-compose`:

```yml
postgres:
  image: public.ecr.aws/f3w3g2g6/docker-postgres-plv8:13.2-2.3.15
```
### Selecting A Version

Points to the latest release available of Postgres `<postgresVersion>` on AWS RDS with the latest release available of plv8 `<plv8Version>` installed.

#### 9.*
`unidays/postgres-plv8:<postgresVersion>-<plv8Version>`
#### 13.*

`public.ecr.aws/f3w3g2g6/docker-postgres-plv8:<postgresVersion>-<plv8Version>`

## License

MIT

[ecr-hub-url]: https://gallery.ecr.aws/f3w3g2g6/docker-postgres-plv8
