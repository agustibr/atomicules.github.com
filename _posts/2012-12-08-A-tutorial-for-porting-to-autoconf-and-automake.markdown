---
layout: page
type: link
title: A Tutorial for Porting to Autoconf and Automake
link: http://mij.oltrelinux.com/devel/autoconf-automake/
categories: 
- program
---
I wanted to make a [very small tweak to a C program](https://github.com/atomicules/pwman/commit/9dadcccb08b770405f4bc15118e914cb3d1946e3) that I use and figured it should be easy enough (it was), but was flummoxed when it came to building the modified source because there was no `configure` file. I'm used to the usual build from source routine of `./configure`, `make`, `make install`, but had never had to deal with a lack of a configure file before. I could have cheated and modified the [release source code (tar.gz)](http://sourceforge.net/projects/pwman/files/pwman/pwman-0.4.4/) (which contains a `configure` file), but I wanted to work with the [repository source](https://pwman.svn.sourceforge.net/svnroot/pwman/trunk/).

I found this page when searching for what on earth to do, which was step 7:

    $ aclocal
    $ automake --add-missing
    $ autoconf

In the process I ["modernised" the build files](https://github.com/atomicules/pwman/commits/modernise) and fixed a load of warnings since the version of `automake` has moved on since the last PWMan release. Nothing clever at all was required on my part here, it was just a matter of using the autohell tools and following the warnings/[instructions](http://www.gnu.org/software/automake/manual/autoconf.html#Obsolete-Constructs).
