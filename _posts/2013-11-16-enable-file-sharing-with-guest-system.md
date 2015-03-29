---
layout: post
title: Enable File Sharing With Guest System in VirtualBox
date: '2013-11-16T23:47:00.001Z'
author: swang
tags: 
modified_time: '2013-11-16T23:49:36.783Z'
thumbnail: http://2.bp.blogspot.com/-flHFOJujZKo/UogEhFLMRUI/AAAAAAAAAJI/ViBsRbylNDI/s72-c/Linux-Clone---Shared-Folders.png
blogger_id: tag:blogger.com,1999:blog-6338160651537141164.post-6122952040038951211
blogger_orig_url: http://swang-ztransform.blogspot.com/2013/11/enable-file-sharing-with-guest-system.html
---

For me the guest system is Arch Linux, so I mostly followed the relevant part in this [link](https://wiki.archlinux.org/index.php/VirtualBox)

On guest, install the VirtualBox Guest Additions:

```Bash
pacman -S virtualbox-guest-utils
```

Manually load the modules with:

```Bash
modprobe -a vboxguest vboxsf vboxvideo
```

Add the new modules to this file:

```Bash
vi /etc/modules-load.d/virtualbox.conf
```

> vboxguest
> vboxsf
> vboxvideo

Start the sharing services:

```Bash
VBoxClient-all &
```

or do the following:

```Bash
sudo systemctl enable vboxservice
sudo systemctl start vboxservice
```

Shared folders are managed via the VirtualBox program on the host. They may be added, auto-mounted and made read-only from there.

![alt text](http://2.bp.blogspot.com/-flHFOJujZKo/UogEhFLMRUI/AAAAAAAAAJI/ViBsRbylNDI/s1600/Linux-Clone---Shared-Folders.png)



If automounting is enabled, and the vboxservice is enabled, creating a shared folder from the VirtualBox program on the host will mount that folder in /media/sf_SHAREDFOLDERNAME on the guest. To have that folder created on the Arch Guest, after the Guest Additions have been installed, you need to add your username to the vboxsf group.

```Bash
groupadd vboxsf
gpasswd -a $USER vboxsf
```

Link the shared folder to your home directory if you want: 

```Bash
ln -s /media/sf_Shared/* ~/Shared 
```

There are also suggestions to add entries in /etc/fstab, I didn't do that and it hasn't caused me any trouble.

