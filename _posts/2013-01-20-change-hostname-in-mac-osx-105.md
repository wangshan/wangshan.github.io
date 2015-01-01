---
layout: post
title: Change hostname in Mac OSX 10.5
date: '2013-01-20T16:15:00.001Z'
author: swang
tags: 
modified_time: '2013-01-20T16:15:30.698Z'
blogger_id: tag:blogger.com,1999:blog-6338160651537141164.post-1774737320154059055
blogger_orig_url: http://swang-ztransform.blogspot.com/2013/01/change-hostname-in-mac-osx-105.html
---

Change in _System Preferences/Sharing_ won't affect the hostname in terminal prompt.

To change it properly, use:

```
sudo scutil --set HostName yourname.local
```

login again you'll see the new hostname by typing

```
hostname
```

