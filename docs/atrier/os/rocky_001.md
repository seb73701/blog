---
title: Install rocky 8
description: 
published: true
date: 2023-04-21T11:16:32.839Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:16:28.662Z
---

```
dnf install vim telnet wget libXt libXmu
```


VBoxClient: error while loading shared libraries: libXt.so.6: cannot open shared object file: No such file or directory modprobe vboxguest failed

so I solved the problem like this:

```
$ sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
$ sudo yum install perl gcc dkms kernel-devel kernel-headers make bzip2
$ reboot
$ mount /dev/cdrom /mnt
$ cd /mnt
$ ./VBoxLinuxAdditions.run
$ reboot
```

```
dnf install libXt libXmu
```