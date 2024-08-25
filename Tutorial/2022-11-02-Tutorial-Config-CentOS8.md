---
layout: post
title: "[Tutorial] Config CentOS 8 Server in 2022"
description: "Config CentOS 8 Server in 2022"
categories: [Tutorial]
tags: [Linux, CentOS8, CentOS]
last_updated: 2022-11-02 23:23:00 GMT+8
excerpt: "Config CentOS 8 Server in 2022"
redirect_from:
  - /2022/11/02/
---

* Kramdown table of contents
{:toc .toc}

# Config CentOS 8 Server in 2022

## Modify the mirror list

After create a new CentOS 8 server instance, if you wanna install anything, you'll get this error:

```
Cannot prepare internal mirrorlist: No URLs in mirrorlist
```

As the mirror list are removed in CentOS 8.

Modify the mirror list:

```bash
sed -i -e "s|mirrorlist=|#mirrorlist=|g" /etc/yum.repos.d/CentOS-*
sed -i -e "s|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g" /etc/yum.repos.d/CentOS-*
```

## Install Gnome (GUI)[^1]

```bash
dnf group list
dnf groupinstall "Server with GUI"
systemctl set-default graphical
systemctl start gdm.service
systemctl enable gdm.service
```

## Install Commonly Used Packages

```bash
yum install wget git
```

# Reboot Fail

Reboot fails after `dnf groupinstall "Server with GUI"`.

# Change the Default Kernel

To change the default kernel version by:

```bash
grubby --set-default /boot/vmlinuz-4.18.0-348.7.1.el8_5.x86_64
reboot
```

Then remove the old kernels:

```bash
rpm -e kernel-modules-4.18.0-240.1.1.el3.x86_64
rpm -e kernel-core-4.18.0-240.1.1.el3.x86_64
```

# Reference

[^1]: https://www.itzgeek.com/how-tos/linux/centos-how-tos/how-to-install-gnome-gui-on-rhel-8.html
