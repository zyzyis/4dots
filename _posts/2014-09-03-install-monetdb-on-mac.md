---
published: true
title: Install MonetDB on Mac OS 10.9.4 from source code
layout: post
---

### > Download the source code

Mercurial hg is the tool to clone the MonetDB codebase. In most cases `hg` is installed by default in a Mac. If not, you can first install Xcode from AppStore and it will bring hg in your disk.

Now the first step is to clone the source code from MonetDB repository.

```
hg clone http://dev.monetdb.org/hg/MonetDB/
```

### > Prepare

Mac OS still has lots of difference than a real Linux development environment. We need to prepare lots of build tools to get it working. Fortunately we have package manager like [brew](http://brew.sh/) which can help us install the build tool prerequisites. So the first step is to install brew (other package manager like macport, fink may also work), then we need to install

```
brew install libtool gettext pcre
```

### > Start bootstrap

The first step before compiling is to launch `./bootstrap` in MonetDB to figure out the environment. Most likely you will find missing dependencies, and one of them could be

```
configure.ac:2045: error: Could not locate the iconv autoconf
	macros. These are usually located in /usr/share/aclocal/iconv.m4 and
	provided by the gettext package.  If your macros are in a different
	location, try setting the environment variable
	M4DIRS="-I/other/macro/dir" before running ./bootstrap or autoreconf
	again.
```

It is complaining that file `iconv.m4` is missing. The file is actually part of the `gettext` package we just installed from brew. We need to tell MonetDB where it is

```
export M4DIRS="-I/usr/local/Cellar/gettext/0.18.3.2/share/aclocal/"
```

Now there should be no complain.

### > Configure and Make
You are ready to compile, type

```
configure & make
```

to build your MonetDB. If you are lucky you should see a success prompt and finally you can install it by 

```
sudo make install
```

Congratulations! you are done. You can test Monetdb by typing `mserver5` and you should see a screen like this as below.

```
~/Documents/github/MonetDB$ mserver5 
# MonetDB 5 server v11.18.0
# This is an unreleased version
# Serving database 'demo', using 4 threads
# Compiled for x86_64-apple-darwin13.3.0/64bit with 64bit OIDs dynamically linked
# Found 16.000 GiB available main-memory.
# Copyright (c) 1993-July 2008 CWI.
# Copyright (c) August 2008-2014 MonetDB B.V., all rights reserved
# Visit http://www.monetdb.org/ for further information
# Listening for connection requests on mapi:monetdb://127.0.0.1:50000/
# MonetDB/SQL module loaded
>
```

### > Next step

Now you can follow the [tutorial](https://www.monetdb.org/Documentation/UserGuide/Tutorial) and start your journey of using Monetdb.
