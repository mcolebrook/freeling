# Install and compile FreeLing 3.1 source code in Ubuntu 14.04.2 Desktop LTS (32 bits)
[FreeLing] (http://nlp.lsi.upc.edu/freeling) is an open source language analysis tool suite. These installation guide is based both on the official FreeLing [source installation manual] (http://nlp.lsi.upc.edu/freeling/doc/userman/html/node12.html) and on my personal experience.

## Run "Software Updater"
First of all, update your system using the "Software Updater", and then type:

``` shell
$ sudo apt-get update
```

## Install development tools and required libraries
The main difference with the official installation guide is including `zlib1g-dev`.

``` shell
$ sudo apt-get install build-essential automake autoconf
$ sudo apt-get install libboost-regex-dev libicu-dev zlib1g-dev
$ sudo apt-get install libboost-system-dev libboost-program-options-dev
```

## Download and Install FreeLing source code
You can download the latest FreeLing version from [here](http://devel.cpl.upc.edu/freeling/downloads?order=time&desc=1). We shall use in this guide version 3.1.

```shell
$ tar xzvf freeling-3.1.tar.gz
$ cd freeling-3.1
```
## Configuring
Within the FreeLing directory, type:

``` shell
$ ./configure
```

In case you get the following error:
```shell
checking for zlib.h... no

   zlib.h not found.
   Make sure zlib headers (package zlib-dev or the like)
   are installed and can be found in a standard path.
   You may need to set CPPFLAGS to specify search path.
```

you can do `$ sudo apt-get install zlib1g-dev`



