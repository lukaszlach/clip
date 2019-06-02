# lukaszlach / clips / showcontext

Display the contents of the build context.

![](https://raw.githubusercontent.com/lukaszlach/clip/master/clips/showcontext/record.gif)

```
docker showcontext DIR
```

Docker does not show the contents of the build context it archives and sends to the Docker Engine during a build process. This plugin allows you to list all the files that would be archived by a `docker build` command pointing the same build context directory in `DIR`. All patterns stored in the `.dockerignore`, if present, file are also properly handled.

## Install

```bash
$ docker clip add lukaszlach/clips:showcontext
```

## Usage

If you are in the project directory you can call `docker showcontext` to list all the files:

```bash
$ docker showcontext .
Building the image layer
Build context contents:
drwxr-xr-x  0 root   root        0 Jun  2 12:58 .git/
-rw-r--r--  0 root   root       21 Jun  1 18:49 .git/COMMIT_EDITMSG
-rw-r--r--  0 root   root       88 Jun  2 12:48 .git/FETCH_HEAD
-rw-r--r--  0 root   root       23 Jun  1 17:30 .git/HEAD
-rw-r--r--  0 root   root       41 Jun  2 12:48 .git/ORIG_HEAD
-rw-r--r--  0 root   root      378 Jun  1 17:33 .git/config
-rw-r--r--  0 root   root       73 Jun  1 17:30 .git/description
drwxr-xr-x  0 root   root        0 Jun  1 17:30 .git/hooks/
...
```

You can see the `.git` directory is included in the build context, exclude it and check again:

```bash
$ echo .git >> .dockerignore
$ docker showcontext .
Building the image layer
Build context contents:
-rw-r--r--  0 root   root       39 Jun  2 12:59 .dockerignore
-rw-r--r--  0 root   root     1086 Jun  2 12:52 LICENSE.md
-rw-r--r--  0 root   root     6130 Jun  2 12:48 README.md
drwxr-xr-x  0 root   root        0 Jun  2 12:36 clips/
drwxr-xr-x  0 root   root        0 Jun  2 12:48 clips/dive/
-rw-r--r--  0 root   root      280 Jun  1 17:41 clips/dive/Makefile
```