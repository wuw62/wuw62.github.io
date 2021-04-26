---
title: A quick fix for insufficient /var space in Linux
categories:
 - Linux
tags:
 -linux
 -unix
date: 2021-04-23 15:45:20
---

This applies to Linux Redhat or maybe some other Linux versions too. I encountered this issue when I was working on a website migration project. Mysql was unable to restart and the error log shows bin log. It turned out to be /var out of space.

## Before

```linux
[wuw62@cpanel /]$ df -h
Filesystem            Size  Used Avail Use% Mounted on
devtmpfs              7.8G     0  7.8G   0% /dev
tmpfs                 7.8G     0  7.8G   0% /dev/shm
tmpfs                 7.8G  121M  7.7G   2% /run
tmpfs                 7.8G     0  7.8G   0% /sys/fs/cgroup
/dev/mapper/vg0-root   16G  4.2G   12G  27% /
/dev/sda1            1014M  220M  795M  22% /boot
/dev/mapper/vg0-var    16G  16G    13M 100% /var
/dev/mapper/vg0-home  256G   24G  233G  10% /home
tmpfs                 1.6G     0  1.6G   0% /run/user/13948

```

## Solution
Use symbolic link(soft link). It only generate a mirror image of /var  and will not occupy disk space, command: ln -s xxx


```linux
mv /var/www /home   #move /www to /home which has 256G space
ln －s  /home/www /var  #/var/www link to /home/www，so /www won't occupy any space of /var

```

## After

```linux
[wuw62@cpanel /]$ df -h
Filesystem            Size  Used Avail Use% Mounted on
devtmpfs              7.8G     0  7.8G   0% /dev
tmpfs                 7.8G     0  7.8G   0% /dev/shm
tmpfs                 7.8G  121M  7.7G   2% /run
tmpfs                 7.8G     0  7.8G   0% /sys/fs/cgroup
/dev/mapper/vg0-root   16G  4.2G   12G  27% /
/dev/sda1            1014M  233M  782M  23% /boot
/dev/mapper/vg0-var    16G  2.8G   14G  18% /var
/dev/mapper/vg0-home  256G   37G  220G  15% /home
tmpfs                 1.6G     0  1.6G   0% /run/user/13948
```

Problem solved. Mysql can be restarted then. 
