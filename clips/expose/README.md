# lukaszlach / clips / expose

Expose your container on the Internet, anytime, for any time.

![](https://raw.githubusercontent.com/lukaszlach/clip/master/clips/expose/record.gif)

```
docker expose [OPTIONS] CONTAINER PORT
```

This Docker Client Plugin allows to expose any HTTP service listening in the container publicly, even if your service if listening only on HTTP - you will get a public HTTPs endpoint. Traffic is proxied underneath through [ngrok](https://ngrok.com) cloud.

## Install

```bash
$ docker clip add lukaszlach/clips:expose
```

## Usage

First you need to have the container you want to expose running. As an example we will run a nginx web server with no additional setup and no port publishing:

```bash
$ docker run --name nginx -d nginx
```

Call `docker expose` to publish port `80` of `nginx` container on the Internet:

```bash
$ docker expose nginx 80
Creating network
Starting proxy for amazing_torvalds:80
Proxy is running, fetching public address details
http://e13de2d6.ngrok.io
https://e13de2d6.ngrok.io
Successfully exposed amazing_torvalds:80
```

Stop the container when the proxy is no longer needed:

```bash
$ docker stop clip-expose
```

Logs show HTTP and HTTPs endpoints you need to use to access your service.

## Docker Registry

As Docker Registry is a web service, you can use `docker expose` to make the registry available on the Internet. First you need to start the Docker Registry:

```bash
$ docker volume create registry-data
$ docker run -d --name registry \
    -v registry-data:/var/lib/registry \
    registry:2
```

Expose the service:

```bash
$ docker expose registry 5000
Creating network
Starting proxy for registry:5000
Proxy is running, fetching public address details
http://c8b0bcb0.ngrok.io
https://c8b0bcb0.ngrok.io
Successfully exposed registry:5000
```

As HTTPs is supported the public registry works out-of-box:

```bash
$ docker tag busybox c8b0bcb0.ngrok.io/busybox
$ docker push c8b0bcb0.ngrok.io/busybox
  The push refers to repository [c8b0bcb0.ngrok.io/busybox]
  d1156b98822d: Pushing [====================>                  ]  1.016MB
```