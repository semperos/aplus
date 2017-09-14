
## Introduction
   see http://www.aplusdev.org/  
   This file documents a successful build of A+ on Fedora-26 with XEmcacs 21-4.34.  
   This documentation may be incomplete (I'm writing it after the fact, and the process was not straightforward).

## Preparation
   This repository was created by pulling down the source code (A+ 4.22-4) from  http://www.aplusdev.org/Download/index.html  
   11 files were modified in order to get a successful build.

## Build & Install 
``` git clone https://github.com/tavmem/aplus
    cd aplus
    CFLAGS=-O2 CXXFLAGS=-O2
    ./configure --prefix=/usr/local
    make
    make install
```
   Verify that the build is successful,
```
   cd /usr/local/aplus/bin
   ./a+
```
   enter some A+ statement like "+/1 2 3"


## Fonts for FireFox
   Go to  http://www.aplusdev.org/Download/index.html   ...  (near the end of the page).   
   Download the True Type fonts for Windows.  It works (you can test it before the download).
```
   mkdir ~/.fonts
   cp ~/Download/KAPL.TTF ~/.fonts/   
```

## XEmacs Setup
   The Fedora-26 package for XEmacs includes MULE, and the APL key mapping fails.  
   You need to download and build the source for XEmacs 21.5.34 without MULE.  
   For a successful build, you wll need to install X11-bitmaps
```
   sudo dnf install xorg-x11-xbitmaps
```
   I needed to make minor changes to
```
     lisp.h
     termcap.c
     gmalloc.c
```
   Once the build is good, create the following file:
```
   cat ~/.xemacs/init.el
   (load "/usr/local/aplus/lisp.0/aplus")
```
   Note that it is lisp.0 (not lisp.1)  
   You will need to make some minor modifications to  
   /usr/local/aplus/lisp.0/aplus.el  

   You need to create a directory for the KAPL fonts:
```
   sudo mkdir /usr/local/share/lib/X11/fonts/misc
   cp ~/aplus/src/fonts/X11/pcf/K* /usr/local/share/lib/X11/fonts/misc/
   cd /usr/local/share/lib/X11/fonts/misc/
   mkfontdir
   cp Kapl.alias fonts.alias
```
   Then you need to notify X11 of the additional fonts (and where to find them).  
   This notification needs to be done whenever Fedora-26 is restarted.
```
   cd /usr/local/share/lib/X11/fonts/misc
   xset fp+ /usr/local/share/lib/X11/fonts/misc
   xset fp rehash
```
