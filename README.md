
## Introduction
	see http://www.aplusdev.org/
	This file documents a successful build of A+ on Fedora-26 with XEmcacs 21-4.34
	The documentation may by incomplete (this is after the fact, and the process was not straithforward).

## Preparation
1. pull down the source code (A+ 4.22-4) from  http://www.aplusdev.org/Download/index.html

## Build & Install 
```shell
    $ git clone https://github.com/tavmem/aplus
    cd aplus
    ./configure --prefix=/usr/local
    make
    make install
```
1. verify that the build is successful,
```shell
   cd /usr/local/aplus/bin
   ./a+
   enter some A+ statement like "+/1 2 3"
```

## fonts for FireFox
   Go to  http://www.aplusdev.org/Download/index.html   ...  at the end
     the True Type fonts for Windows woks  (you can test it)
   Download
   mkdir ~/.fonts
   cp ~/Download/KAPL.TTF ~/.fonts/   

## XEmacs Setup
   You wll need to install
	sudo dnf install xorg-x11-xbitmaps
   The Fedora-26 package for XEmacs incudes MULE (which obliterates the APL key mapping)
        You ned to download and build the source for XEmancs 21.5.34
        You will need to make changes to
           lisp.h
	   termcap.c
	   gmalloc.c

   Create the following file:
        $ cat ~/.xemacs/init.el
        (load "/usr/local/aplus/lisp.0/aplus")
   Note that it is lisp.0 (not lisp.1)

   You need to create a directory:
	sudo mkdir /usr/local/share/lib/X11/fonts/misc

   The following commands need to be executed everytime Fedora-26 is restated:
	cd /usr/local/share/lib/X11/fonts/misc
	xset fp+ /usr/local/share/lib/X11/fonts/misc
	xset fp rehash
