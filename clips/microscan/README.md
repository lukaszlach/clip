# lukaszlach / clips / microscan

Scan the image with [Aqua Microscanner](https://github.com/aquasecurity/microscanner).

```
docker microscan --token TOKEN IMAGE
```

## Install

```bash
$ docker clip add lukaszlach/clips:microscan
```

## Usage

Scan any image with the `docker microscan` command, if it is not available locally it will be pulled automatically. The newest version of Aqua Microscanner is downloaded on every scan (~30MB) so that it is always valid and able to communicate with the security database.

```bash
$ docker microscan --token MTdmYcM5ODa1NmMd alpine:3.9
...
  "vulnerability_summary": {
    "total": 0,
    "high": 0,
    "medium": 0,
    "low": 0,
    "negligible": 0,
    "sensitive": 0,
    "malware": 0
  },
...
No critical vulnerabilities found in alpine:3.9

$ docker microscan --token MTdmYcM5ODa1NmMd debian:stretch
```

> To use MicroScanner you'll first need to [register](https://github.com/aquasecurity/microscanner#registering-for-a-token) for a token.

You can also scan the image from a local Docker Registry:

```bash
$ docker microscan --token MTdmYcM5ODa1NmMd \
    local.registry.com/image:tag
```

## Support

Plugin needs to install `ca-certificates` inside the container before the scanning process.
Below package managers are supported:

* apt (Debian, Ubuntu)
* apk (Alpine)
* dnf (Fedora)
* yum (CentOS)
