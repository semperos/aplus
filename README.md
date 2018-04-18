
## Introduction
   see http://www.aplusdev.org/  
   This file documents a successful build of A+ on Fedora-26 with XEmcacs 21-4.34. 

## Preparation
   This repository was created by pulling down the source code (A+ 4.22-4) from  http://www.aplusdev.org/Download/index.html  
   11 files were modified in order to get a successful build.

## Build & Install 
```
    git clone https://github.com/tavmem/aplus
    cd aplus
    CFLAGS=-O2 CXXFLAGS=-O2
    ./configure --prefix=/usr/local --libdir=/lib64
    make
    sudo make install
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
   The APL key mapping fails when using the Fedora-26 package for XEmacs.  
   My guess is that the package includes MULE “MUlti-Lingual Emacs”.  
   I downloaded and built the source for XEmacs 21.5.34 without MULE (and that worked).  
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
   In lisp.h the structure max_align_t was already defined, so I commented out the re-definition: 
```
/*
typedef union
{
  struct { long l; } l;
  struct { void *p; } p;
  struct { void (*f)(void); } f;
  struct { double d; } d;
} max_align_t;
*/

```
   In termcap.c there was no definition for speed_t, so I added one:
```
$ diff ~/xemacs-21.5.34/src/termcap.c ./termcap.c
43a44,45
> typedef unsigned int    speed_t;
> 
```
   In gmalloc.c I commented out one definition:
```
$ diff ~/xemacs-21.5.34/src/gmalloc.c ./gmalloc.c
1203c1203
< #define	__sbrk	sbrk
---
> /* #define	__sbrk	sbrk */
``` 
   Building XEmacs
```
   sudo mkdir /usr/local/src/xemacs
   cd /usr/local/src/xemacs  
   sudo tar xzf ~/Downloads/xemacs-21.5.34.tar.gz
   ./configure 
   sudo make  
   sudo make install
```
   Followed by installing the packages
```
   sudo mkdir -p /usr/local/share/xemacs
   cd /usr/local/share/xemacs
   sudo tar xzf /tmp/xemacs-sumo.tar.gz
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
   sudo cp ~/aplus/src/fonts/X11/pcf/K* /usr/local/share/lib/X11/fonts/misc/
   cd /usr/local/share/lib/X11/fonts/misc/
   sudo mkfontdir
   sudo cp Kapl.alias fonts.alias
```
   Then you need to notify X11 of the additional fonts (and where to find them).  
   This notification needs to be done whenever Fedora-26 is restarted.
```
   cd /usr/local/share/lib/X11/fonts/misc
   xset fp+ /usr/local/share/lib/X11/fonts/misc
   xset fp rehash
```
