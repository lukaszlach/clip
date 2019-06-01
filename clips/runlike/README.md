# lukaszlach / clips / runlike

Call [runlike](https://github.com/lavie/runlike) on any container to get the command line necessary to run a copy of it.

```
docker runlike CONTAINER
```

## Install

```bash
$ docker clip add lukaszlach/clips:runlike
```

## Usage

If you have the container running Alpine:

```bash
$ docker run -d --name alpine alpine:3.9 sleep 1h
```

You can use `docker runlike` to get the command needed to create an exact copy of the container. In case the container was ran with docker-compose you will get the command you have never had a chance to see.

```bash
$ docker runlike alpine
docker run --name=alpine --hostname=19fd796d28e6 --env="PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin" --restart=no --detach=true alpine:3.9 sleep 1h
```