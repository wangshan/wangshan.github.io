---
layout: post
title: How to Setup a Command Line Based Mac Development Environment
date: '2012-03-03T19:49:00.000Z'
author: swang
tags: 
modified_time: '2013-08-26T00:14:15.989+01:00'
blogger_id: tag:blogger.com,1999:blog-6338160651537141164.post-2607141862908425358
blogger_orig_url: http://swang-ztransform.blogspot.com/2012/03/mac-development-environment-setup.html
---

Here is what I did to my 2013 macbook (OSX 10.10.1) to get the developing environment I want.


### Homebrew

Install:

```ruby
/usr/bin/ruby -e "$(curl -fsSL https://raw.github.com/gist/323731)"
```

Result:

```
Warning: The following *evil* dylibs exist in /usr/local/lib

They may break builds or worse. You should consider deleting them:

/usr/local/lib/libfuse.2.dylib

/usr/local/lib/libfuse_ino64.2.dylib

==> Installation successful!
```

Now type `brew help` to familiar yourself with homebrew


### [tmux](http://tmux.sourceforge.net/)

depends on libevent, install by homebrew:

```
brew install libevent
```


### Font

get them from [here](http://www.lowing.org/fonts/)

Used ones:
* Bitstream Vera Sans Mono
* Monaco


### pip, virtualenv, virtualenvwrapper

```
sudo easy_install pip
sudo pip install virtualenv
sudo pip install virtualenvwrapper
```

to use a specific version of python in a virtual environment:

```
virtualenv --python=/usr/local/bin/python2.7 python27
```
  
to activate:

```
source python27/bin/activate
```
    
to exit, in the virtualenv, type `deactivate`
     
to remove a virtual environment, simply delete the created folder:

```
rm -rf python27
```


### [autojump](https://github.com/joelthelion/autojump/wiki)

### [zsh](http://zshwiki.org/home/)

### [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)

### Install gcc 4.8 with colorgcc and ccache

```
brew tap homebrew/versions
brew install gcc48
```

download colorgcc.pl from [here](https://github.com/colorgcc/colorgcc)
save it to /usr/local/bin

delete gcc and g++ links in /usr/bin, then

```
sudo ln -s /usr/local/bin/colorgcc.pl gcc
sudo ln -s /usr/local/bin/colorgcc.pl g++
```

```
brew install ccache
```

create /usr/local/bin/ccache-gcc.sh and /usr/local/bin/ccache-g++.sh

then

```
ccache /usr/local/bin/gcc-4.8 "$@" 

ccache /usr/local/bin/g++-4.8 "$@" 
```

edit colorgccrc to make it use cache

```
vi ~/.colorgccrc
```
change the following line:

_gcc: /usr/local/bin/ccache-gcc.sh_

_g++: /usr/local/bin/ccache-g++.sh_


### Haskell

```
brew install haskell-platform
```


#### Note: the gcc command on Mac OSX is no longer a real gcc, it's just a gcc front end for clang, so even it mentions gcc version 4.2.1, it's actually clang-600, which has all the C++11 features you need supported.
