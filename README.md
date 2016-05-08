# pcb2gcode [![Build Status](https://travis-ci.org/pcb2gcode/pcb2gcode.svg?branch=master)](https://travis-ci.org/pcb2gcode/pcb2gcode)

pcb2gcode is a command-line software for the isolation, routing and drilling of PCBs.
It takes Gerber files as input and it outputs gcode files, suitable for the milling of PCBs.
It also includes an Autoleveller, useful for the automatic dynamic calibration of the milling depth.

This is a complete rewrite of the original pcb2gcode in C++.

## Quick Installation
This development version of pcb2gcode does not get into repositories of distros. If you want to test this version, you will have to go to the section below (installation from GIT).

#### Archlinux:
* pcb2gcode stable -> [`https://aur.archlinux.org/packages/pcb2gcode/`](https://aur.archlinux.org/packages/pcb2gcode/)
* pcb2gcode git -> [`https://aur.archlinux.org/packages/pcb2gcode-git/`](https://aur.archlinux.org/packages/pcb2gcode-git/)

#### Fedora:
* Download the latest tarball from https://github.com/pcb2gcode/pcb2gcode/releases
* Open a terminal and cd to the extracted tarball
* type the following:

    ```
    su
    <the root password>
    yum groupinstall "Development Tools"
    yum install automake autoconf libtool boost-devel gtkmm24-devel gerbv-devel
    exit
    ./configure
    make
    su -c 'make install'
	```
    
* done.

#### Debian, Ubuntu:
There are pcb2gcode packages in the official repositories. You can install the with

    sudo apt-get install pcb2gcode

Unfortunately, these packages are outdated. If you want to download the latest development
version, go to "Installation from GIT".
I'm currently looking for a Debian/Ubuntu package maintainer: if you're interested [write me an email](mailto:nicola@corna.info).

#### Windows
Windows prebuilt binaries (with all the required DLLs) are available in the [release](https://github.com/pcb2gcode/pcb2gcode/releases) page.

#### Mac OS X
pcb2gcode is available in [Homebrew](http://brew.sh/). To install it open the "Terminal" app and run the following commands; pcb2gcode and the required dependencies will be automatically downloaded and installed:

     $ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
     $ brew install pcb2gcode

## Installation from GIT (latest development version):
If you want to install the latest version from git you'll need the autotools, boost with the program_options library
(dev, >= 1.47), gtkmm2.4 (dev) and libgerbv (dev).

#### Ubuntu 12.04 LTS

    $ sudo apt-get update
    $ sudo apt-get install software-properties-common python-software-properties
    $ sudo add-apt-repository "ppa:ubuntu-toolchain-r/test"
    $ sudo apt-get update
    $ sudo apt-get install gcc-4.8 g++-4.8
    $ export CC=gcc-4.8 CXX=g++-4.8

Then follow the steps for [Ubuntu Trusty](#debianlike)

#### Debian Wheezy or newer, Ubuntu Trusty or newer<a name="trusty"></a>

    $ sudo apt-get update
    $ sudo apt-get install build-essential automake autoconf autoconf-archive libtool libboost-program-options-dev libgtkmm-2.4-dev gerbv git
    $ git clone https://github.com/pcb2gcode/pcb2gcode.git
    $ cd pcb2gcode

Then follow the [common build steps](#commonbuild)

#### Fedora

    su
    <the root password>
    yum groupinstall "Development Tools"
    yum install automake autoconf libtool boost-devel gtkmm24-devel gerbv-devel git
    exit

Then follow the [common build steps](#commonbuild)

#### Common build steps<a name="commonbuild"></a>

    $ autoreconf -i
    $ ./configure
    $ make
    $ sudo make install

### Windows
You can easily build pcb2gcode for Windows with MSYS2 (http://sourceforge.net/projects/msys2/).
Download MSYS2 and install it somewhere, then run "MinGW-w64 Win32 Shell" (if you want a i686 binary) or "MinGW-w64 Win64 Shell" (if you want a x86_64 binary). The following commands are for the i686 binary, if you want the x86_64 binary replace all the "/mingw32" with "/mingw64" and all the mingw-w64-i686-* packages with mingw-w64-x86_64-*

    $ pacman -Sy
    $ pacman --needed -S bash pacman pacman-mirrors msys2-runtime

Close and reopen the shell

    $ pacman -Su
    $ pacman --needed -S base-devel git mingw-w64-i686-gcc mingw-w64-i686-boost mingw-w64-i686-gtkmm

Now let's download, build and install gerbv (version 2.6.1 is broken, don't use it)

    $ wget downloads.sourceforge.net/gerbv/gerbv-2.6.0.tar.gz
    $ tar -xzf gerbv-2.6.0.tar.gz
    $ cd gerbv-2.6.0/    
    $ ./configure --prefix=/mingw32 --disable-update-desktop-database
    $ make
    $ make install

Finally, download and build pcb2gcode

    $ cd ..
    $ git clone https://github.com/pcb2gcode/pcb2gcode.git
    $ cd pcb2gcode/
    $ autoreconf -i
    $ ./configure --prefix=/mingw32
    $ make LDFLAGS='-s'

The dynamically linked binary is &lt;msys2 installation folder&gt;/home/&lt;user&gt;/pcb2gcode/.libs/pcb2gcode.exe.
You can find all the DLLs in &lt;msys2 installation folder&gt;/mingw32/bin; copy them in the same folder of pcb2gcode. The required DLLs are:
 * libatk-1.0-0.dll
 * libboost_program_options-mt.dll
 * libbz2-1.dll
 * libcairo-2.dll
 * libcairomm-1.0-1.dll
 * libexpat-1.dll
 * libffi-6.dll
 * libfontconfig-1.dll
 * libfreetype-6.dll
 * libgcc_s_dw2-1.dll (for the i686 binary)
 * libgcc_s_seh-1.dll (for the x86_64 binary)
 * libgdkmm-2.4-1.dll
 * libgdk_pixbuf-2.0-0.dll
 * libgdk-win32-2.0-0.dll
 * libgerbv-1.dll
 * libgio-2.0-0.dll
 * libglib-2.0-0.dll
 * libglibmm-2.4-1.dll
 * libgmodule-2.0-0.dll
 * libgobject-2.0-0.dll
 * libgtk-win32-2.0-0.dll
 * libharfbuzz-0.dll
 * libiconv-2.dll
 * libintl-8.dll
 * libpango-1.0-0.dll
 * libpangocairo-1.0-0.dll
 * libpangoft2-1.0-0.dll
 * libpangomm-1.4-1.dll
 * libpangowin32-1.0-0.dll
 * libpixman-1-0.dll
 * libpng16-16.dll
 * libsigc-2.0-0.dll
 * libstdc++-6.dll
 * libwinpthread-1.dll
 * zlib1.dll

#### Mac OS X
You can build the latest pcb2gcode version with [Homebrew](http://brew.sh). If Homebrew is not installed yet, install it with the following command:

     $ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
     
Then you can download and build the git version with
     
     $ brew install --HEAD pcb2gcode

or (if pcb2gcode is already installed)

     $ brew upgrade --HEAD pcb2gcode

For further details, see INSTALL.
