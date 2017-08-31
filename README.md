# Minicom (Snap port)

Author: [Joe Testa](http://www.positronsecurity.com/company/) ([@therealjoetesta](https://twitter.com/therealjoetesta))

## Overview

This is a [snap](https://snapcraft.io/) port of the [minicom program](https://en.wikipedia.org/wiki/Minicom).  It works well in *devmode*, but does not yet work in *strict* mode on the Raspberry Pi 2 due to a missing gadget slot for serial ports.  It may or may not work on other platforms.


## Build Process

1.) Begin by obtaining the sources for ncurses (note that a nightly build is used because the latest stable version does not compile under Ubuntu Core 16):

    wget http://invisible-mirror.net/archives/ncurses/current/ncurses-6.0-20170722.tgz
    wget http://invisible-mirror.net/archives/ncurses/current/ncurses-6.0-20170722.tgz.asc

2.) Check the signature:

    gpg --recv-key F7E48EDB
    gpg --verify ncurses-6.0-20170722.tgz.asc ncurses-6.0-20170722.tgz

3.) Get the minicom sources:

    wget https://alioth.debian.org/frs/download.php/file/4215/minicom-2.7.1.tar.gz

Unfortunately, the author has not provided a signature.  However, you can check it against this SHA256 hash: *532f836b7a677eb0cb1dca8d70302b73729c3d30df26d58368d712e5cca041f1*

4.) Apply patches.  These are necessary for the code to compile and run in the special snap environment:

    tar xzf ncurses-6.0-20170722.tgz
    tar xzf minicom-2.7.1.tar.gz
    pushd ncurses-6.0-20170722; patch -p1 < ../ncurses-6.0-20170722_snap.patch; popd
    pushd minicom-2.7.1; patch -p1 < ../minicom-2.7.1_snap.patch; popd

5.) Build the snap:

    snapcraft
