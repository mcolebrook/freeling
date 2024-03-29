# Install and compile FreeLing 3.1 source code in Ubuntu 14.04.2 Desktop LTS (32 bits) to use it with Java
[FreeLing] (http://nlp.lsi.upc.edu/freeling) is an open source language analysis tool suite. These installation guide is based both on the official FreeLing [source installation manual] (http://nlp.lsi.upc.edu/freeling/doc/userman/html/node12.html) and on my personal experience.

## Run "Software Updater"
First of all, update your system using the "Software Updater", and then type:

```
$ sudo apt-get update
```

## Install development tools and required libraries
The main difference with the official installation guide is including `zlib1g-dev`.

```
$ sudo apt-get install build-essential automake autoconf
$ sudo apt-get install libboost-regex-dev libicu-dev zlib1g-dev
$ sudo apt-get install libboost-system-dev libboost-program-options-dev
```

## Download and Install FreeLing source code
You can download the latest FreeLing version from [here](http://devel.cpl.upc.edu/freeling/downloads?order=time&desc=1). We shall use in this guide version 3.1.

```
$ tar xzvf freeling-3.1.tar.gz
$ cd freeling-3.1
```
## Configuring
Within the FreeLing directory, type:

```
$ ./configure
```

In case you get the following error:
```
checking for zlib.h... no

   zlib.h not found.
   Make sure zlib headers (package zlib-dev or the like)
   are installed and can be found in a standard path.
   You may need to set CPPFLAGS to specify search path.
```

you can type:
```
$ sudo apt-get install zlib1g-dev
```

In case you get the following message:
```
checking for main in -lboost_thread-gcc-mt... no

   boost_thread library not found.
   Make sure libboost_thread is
   installed and can be found in a standard path.
   You may need to set LDFLAGS to specify search path.
```

the solution is typing:

```
$ sudo apt-get install libboost-thread-dev
```

## Building the binaries
Inside the FreeLing folder, just type:
```
$ make
```

Should you get the following error:

```
corrector/dicc2phon-dicc2phon.o: In function `_GLOBAL__sub_I_main':
dicc2phon.cc:(.text.startup+0x2c): undefined reference to `boost::system::generic_category()'
dicc2phon.cc:(.text.startup+0x36): undefined reference to `boost::system::generic_category()'
dicc2phon.cc:(.text.startup+0x40): undefined reference to `boost::system::system_category()'
collect2: error: ld returned 1 exit status
```

the solution is running again the `./configure` script with the following variables set:

```
CXXFLAGS=-lboost_system CPPFLAGS=-lboost_system LIBS=-lboost_system ./configure
```

and re-run the `make` command:

```
$ make
```

For more information on this topic, please refer to the following threads in the FreeLing Forum:
  * [-lboost_system option is missing](http://nlp.lsi.upc.edu/freeling/index.php?option=com_simpleboard&Itemid=&func=view&catid=5&id=4013#4013)
  * [Freeling 3.1 -- Ubuntu 14.04](http://nlp.lsi.upc.edu/freeling/index.php?option=com_simpleboard&Itemid=65&func=view&id=3872&catid=5)

## Installing
Once the source is compiled, you can install it using:

```
$ sudo make install
```

FreeLing library is entirely contained in the file `libfreeling.so` which is located in `/usr/local/lib` by default. Sample program `analyze` is installed in `/usr/local/bin`.

## Testing FreeLing

You may check FreeLing by typing the following lines:
```
$ analyze -f es.cfg
Hola FreeLing, este es mi primer mensaje.
Hola hola I 1
FreeLing freeling NP00000 1
, , Fc 1
este este PD0MS000 0.0314658
es ser VSIP3S0 1
mi mi DP1CSS 0.999101
primer 1 AO0MS0 1
mensaje mensaje NCMS000 1
. . Fp 1
^C
```

## Installing Java JDK
First, move to `freeling-3.1/APIs/java` and read the file `README`. Then, type:
```
$ sudo apt-get install openjdk-7-jdk
```

## Installl SWIG
In order to call the FreeLing library from Java, first we must install SWIG:
```
$ sudo apt-get install swig
```

## Edit `freeling-3.1/APIs/java/Makefile`
Now, you have to set the following variables to the right values of your system:

```
# directory prefix where you installed FreeLing (by default in /usr/local)
FREELINGDIR = /usr/local

# directory where swig share files are installed in your system
SWIGDIR = /usr/share/swig2.0

# directory where the JVM is installed
JAVADIR = /usr/lib/jvm/java-7-openjdk-i386
```

Also, we had to add `-lboost_system` to the following line to avoid compiling errors:

```
$(GCC) -shared -o libfreeling_javaAPI.so freeling_javaAPI.cxx -lfreeling -lboost_system -L$(FREELINGDIR)/lib -I$(FREELINGDIR)/include -I$(JAVADIR)/include -I$(JAVADIR)/include/linux -fPIC
```

## Compile
Just type:

```
$ make
```

## Set the env variables
You have to set the following environment variables to the right values of your installation:

```
# directory where you have put the FreeLing folder (in my case, I moved it to /opt/freeling-3.1)
$ export FREELINGDIR=/opt/freeling-3.1

# add to LD_LIBRARY_PATH the following directories which contain libfreeling.so and libfreeling_javaAPI.so
$ export LD_LIBRARY_PATH=/usr/local/lib:$FREELINGDIR/APIs/java

# path to the package freeling.jar
$ export CLASSPATH=$FREELINGDIR/APIs/java
```

**REMEMBER:** to put these lines in your starting shell scripts: .bashrc, .cshrc, etc.

## Compile and run a Java example
Compile the `Analyzer.java` file:

```
$ javac Analyzer.java
```

Finally, just test the new libraries by typing:
```
$ java Analyzer
Hola, este es un mensaje para el ejemplo de Java.
...
```

**DO NOT FORGET** to finish your line with a full stop `'.'` to avoid empty results.

**CONGRATULATIONS!!** Now you can work with FreeLing using Java!
