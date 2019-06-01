# lukaszlach / clips / publish

Publish a port on a running container using a TCP proxy.

```
docker publish add CONTAINER PORT
docker publish rm PORT
docker publish ls
```

This Docker Client Plugin allows to publish a container port which was not published before, because of reasons. The traffic goes through the [TCP proxy]() running in other container which actually publishes the port.

## Install

```bash
$ docker clip add lukaszlach/clips:publish
```

## Usage

### add

In case you have a container running and need to publish any TCP port, run `docker publish add` command:

```bash
$ docker run --name nginx -d nginx
```

```bash
$ docker publish add nginx 80
b2b7b0f147b3179d315aacb1a25f93526c03614fe8431e9fcabc8d4f318cbd28
Successfully published nginx:80
```

You can also pass a "ip:port" value in order to bind to the specific network interface:

```bash
$ docker publish add nginx 127.0.0.1:80
```

### rm

When the published port is no longer needed you can manually remove it with the `docker publish rm` command. When the parent container on which the proxy depends on is stopped, so is the proxy.

```bash
$ docker publish rm 80
Successfully stopped publishing :80
```

### ls

List all additionally published ports.

```bash
$ docker publish ls
PORT            CONTAINER
80              3f93e8f80bca
```