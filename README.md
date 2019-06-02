# lukaszlach / clip

![Version](https://img.shields.io/badge/version-19.06.0-lightgrey.svg?style=flat)

**Docker Client Plugins Manager** (CLIP) is a Docker client plugin allowing to create, build new [Docker client plugins](https://github.com/docker/cli/issues/1534) in any programming language, publish them on Docker Hub or just try some plugins from the catalog and install them locally. Project is written entirely in Bash.

[![](https://raw.githubusercontent.com/lukaszlach/clip/master/clips/dive/record.gif)](clips/dive/)

Plugins are distributed as Docker images and can be published on Docker Hub, they are built from "scratch" so the image size reflects the plugin binary size. Example plugins from [this repository](clips/) are written in Bash so the plugin images are [tiny](https://cloud.docker.com/repository/docker/lukaszlach/clips/tags) (500 bytes - 2 KB).

Start with downloading the CLIP Docker plugin:

```bash
curl -sf https://raw.githubusercontent.com/lukaszlach/clip/master/docker-clip -o ~/.docker/cli-plugins/docker-clip
chmod +x ~/.docker/cli-plugins/docker-clip
```

It should be automatically detected by the Docker client:

```bash
$ docker info | grep clip
  clip: Docker Client Plugins Manager (Łukasz Lach, v19.06.0)

$ docker clip --help
```

There are a lot of ready plugins available in this repository and already published on Docker Hub. You can [install](#install-the-plugin) them with a single `docker clip add` command. If you do not like one of the plugins you can [remove](#remove-the-plugin) it using the `docker clip rm` command.

* [expose](clips/expose/) - Expose your container on the Internet, anytime, for any time ([see how it works](clips/expose/record.gif))
* [publish](clips/publish/) - Publish a port on a running container
* [microscan](clips/microscan/) - Scan the image with Aqua Microscanner
* [dive](clips/dive/) - Explore contents of the image layers ([see how it works](clips/dive/record.gif))
* [runlike](clips/runlike/) - Get the command line necessary to run a copy of any container
* [sh](clips/sh/) - Enter the container shell no matter if it is bash or sh
* [hello](clips/hello/) - Hello World example plugin

## Install the plugin

```
docker clip add IMAGE:TAG
```

As CLIP distributes plugins as Docker images - all you need to know is the image name and tag. Call `docker clip add` to install any plugin locally, the image will be pulled from the registry if needed.

```bash
$ docker clip add lukaszlach/clips:expose
Installing client plugin from lukaszlach/clips:expose
Successfully installed lukaszlach/clips:expose client plugin
New client command available: 'docker expose'

$ docker expose --help
```

## Remove the plugin

```
docker clip rm COMMAND
```

You can uninstall the plugin with the `docker clip rm` command:

```bash
$ docker clip rm expose
Successfully removed 'microscan' client command
```

## List plugins

```
docker clip ls
```

The command lists all additional Docker client plugins installed and maintained by CLIP:

```bash
$ docker clip ls
COMMAND      IMAGE
microscan    lukaszlach/clips:microscan
```

## Build new plugin

```
docker clip build -c COMMAND -t IMAGE:TAG CONTEXT
docker clip push IMAGE:TAG
```

In order to build a new plugin you need to create a build context directory which must contain two files. Assuming you are developing the `hello` command (see [the example implementation](clips/hello/) in this repository):

* `docker-hello` - the plugin executable, can be a binary or a shell script
* `docker-hello.json` - the plugin manifest in JSON format

The manifest file is used by Docker Client to fetch details about the plugin and to ensure it is supported.

* `SchemaVersion` needs to be set to `0.1.0`
* `Vendor`, `Version` and `ShortDescription` can take any string value, they are visible in the output of `docker` and `docker info` commands

```json
{
  "SchemaVersion": "0.1.0",
  "Vendor": "Łukasz Lach",
  "Version": "v1.0",
  "ShortDescription": "Hello World"
}
```

The script stored in `docker-hello` can be as simple as:

```bash
#!/bin/sh
echo Hello World
```

Any other files present in the build context directory will be added along with the binary and manifest, your plugin can use and depend on these files to be present on the client disk after installation.

After you are ready with the plugin code, build the plugin image with the `docker clip build` command:

```bash
$ docker clip build -c hello -t lukaszlach/clips:hello .
Building lukaszlach/clips:hello client plugin
sha256:8d53da12e07fe6d0ae9f129311de985361718f6903583113082042f96ca95be3
Successfully built lukaszlach/clips:hello client plugin
```

Pass the command name in `-c` parameter and an image tag in `-t`, the last parameter is a build context. The command works similarly to `docker build` and actually uses it underneath to build the image.

You can now test the plugin locally by simply installing it or push it to the registry with the `docker clip push` command:

```bash
$ docker clip push lukaszlach/clips:hello
```

## Licence

MIT License

Copyright (c) 2019 Łukasz Lach <llach@llach.pl>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
