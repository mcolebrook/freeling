# Install and compile FreeLing 3.1 source code in Ubuntu 14.04.2 Desktop LTS (32 bits)
[FreeLing] (http://nlp.lsi.upc.edu/freeling) is an open source language analysis tool suite. These installation guide is based both on the official FreeLing [source installation manual] (http://nlp.lsi.upc.edu/freeling/doc/userman/html/node12.html) and on my personal experience.

## Run "Software Updater"
First of all, update your system using the "Software Updater", and then type:
``` shell
$ sudo apt-get update
```

## Install development tools and required libraries

``` shell
$ sudo apt-get install build-essential automake autoconf
$ sudo apt-get install libboost-regex-dev libicu-dev <b>zlib1g-dev</b>
$ sudo apt-get install libboost-system-dev libboost-program-options-dev

```
