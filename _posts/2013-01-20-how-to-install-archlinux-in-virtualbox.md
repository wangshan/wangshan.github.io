---
layout: post
title: How to install ArchLinux in VirtualBox on Mac OSX 10.6
date: '2013-01-20T16:15:00.000Z'
author: swang
tags: 
modified_time: '2013-11-16T23:50:17.641Z'
blogger_id: tag:blogger.com,1999:blog-6338160651537141164.post-1113281250764819552
blogger_orig_url: http://swang-ztransform.blogspot.com/2013/01/how-to-install-archlinux-in-virtualbox.html
---

**Note this is written in 2012**

### Install

Download core iso file:
archlinux-2011.08.19-core-x86_64.iso

Download checksum:
sha1sums.txt

Install md5sha1sum:

```Bash
brew install md5sha1sum
```

Check integrity:

```Bash
sha1sum --check sha1sums.txt                                                                                             

zsh: correct 'sha1sum' to 'shasum' [nyae]? n
sha1sum: stat 'archlinux-2011.08.19-core-dual.iso': No such file or directory
sha1sum: stat 'archlinux-2011.08.19-core-i686.iso': No such file or directory
archlinux-2011.08.19-core-x86_64.iso: OK
sha1sum: stat 'archlinux-2011.08.19-netinstall-dual.iso': No such file or directory
sha1sum: stat 'archlinux-2011.08.19-netinstall-i686.iso': No such file or directory
sha1sum: stat 'archlinux-2011.08.19-netinstall-x86_64.iso': No such file or directory
sha1sum: stat 'archlinux-2011.08.19-core-dual.iso.torrent': No such file or directory
sha1sum: stat 'archlinux-2011.08.19-core-i686.iso.torrent': No such file or directory
archlinux-2011.08.19-core-x86_64.iso.torrent: OK
sha1sum: stat 'archlinux-2011.08.19-netinstall-dual.iso.torrent': No such file or directory
sha1sum: stat 'archlinux-2011.08.19-netinstall-i686.iso.torrent': No such file or directory
sha1sum: stat 'archlinux-2011.08.19-netinstall-x86_64.iso.torrent': No such file or directory
```

Burn the .iso file to dvd (note: note as a volume but as a iso image)

Create a new virtual machine in virtual box, start it and boot from dvd.

Choose Boot Arch Linux

Then follow the Arch Linux Installation Framework.
I've chosen **ext3** for / and /home

Configuration files (Important!!)

/etc/rc.conf : kept keymap as us
/etc/fstab : filesystem
/etc/resolv.conf : DNS
/etc/hosts
/etc/local.gen : used by /usr/bin/locale-gen to generate locales
/etc/pacman.conf : uncommented [multilib] and all the uk mirrors
/boot/grub/menu.lst : add vga=0x318 to [kernel] line


### After installation:

Refresh package db

```
pacman -Syy
```

Change keyserver in /etc/pacman.d/gnupg/gpg.conf to **hkp://pgp.mit.edu:11371***

Initialize pgp keys:

```
pacman-key --init
```

```Bash
for key in FFF979E7 CDFD6BB0 4C7EA887 6AC6A4C2 824B18E8; do
    pacman-key --recv-keys $key
    pacman-key --lsign-key $key
    printf 'trust\n3\nquit\n' | gpg --homedir /etc/pacman.d/gnupg/ \
        --no-permission-warning --command-fd 0 --edit-key $key
done
```

Sync the entire system had to do this twice as pacman itself needed update

```
pacman -Syu
```

Add non-root user, and add it to wheel group(all according to beginner's guide)

Installed xserver and twm for testing, haven't installed any desktop environment.

Installed Guest Addition for virtualbox, configured ~/.xinitrc

```
cp /etc/skel/xinitrc ~/.xinitrc
```

There must be one exec command not commented in .xinitrc, then add VBoxClient-all & to the first line.

startx works, but without a windows manager it seems impossible to extend xterm to a bigger resolution, even the automatic resize works for the x windows as a whole. There seems no way to have automatic resize in the console. Tried to use gpm for console mouse support, but it's horrible. In the end I decided to install and start sshd, and login to the box via ssh from my mac.

