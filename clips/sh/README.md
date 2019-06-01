# lukaszlach / clips / sh

Enter the container shell no matter if it is bash or sh.

```
docker sh CONTAINER
```

This Docker Client Plugin gives you a shell access to the container. When bash is not available, you get sh.

## Install

```bash
$ docker clip add lukaszlach/clips:sh
```

## Usage

If you have two containers running Alpine and Debian:

```bash
$ docker run -d --name alpine alpine:3.9 sleep 1h
$ docker run -d --name debian debian:stretch sleep 1h
```

You can use `docker sh` to access the shell of both containers, notice the difference:

```bash
$ docker sh alpine
/ # echo $0
 /bin/sh

$ docker sh debian
root@27a6a38d6465:/# echo $0
 /bin/bash
```