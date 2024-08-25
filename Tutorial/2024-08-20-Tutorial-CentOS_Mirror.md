---
layout: post
title: "[Tutorial] Updating CentOS 7 Mirror to Aliyun"
description: "Updating the CentOS 7 mirror to the Aliyun mirror, including updating the CentOS 7 mirror configuration file and updating the mirror cache."
categories: [Tutorial]
tags: [Linux, CentOS]
last_updated: 2024-08-20 18:10:00 GMT+8
excerpt: "Updating the CentOS 7 mirror to the Aliyun mirror, including updating the CentOS 7 mirror configuration file and updating the mirror cache."
redirect_from:
  - /2024/08/20/
---

* Kramdown table of contents
{:toc .toc}

# Problem Description

As the CentOS 7 is no longer maintained, the default mirror is not available. We need to update the CentOS 7 mirror to the Aliyun mirror even we pull the official CentOS 7 image from Docker Hub.

# Updating the CentOS 7 Mirror

```bash
sed -e "s|^mirrorlist=|#mirrorlist=|g" \
    -e "s|^#baseurl=http://mirror.centos.org/centos/\$releasever|baseurl=https://mirrors.aliyun.com/centos-vault/7.9.2009|g" \
    -i.bak \
    /etc/yum.repos.d/CentOS-Base.repo
```

Then run:

```bash
yum clean
yum makecache
```

# Explanation

1. The `sed` command is used to replace the `mirrorlist` and `baseurl` in the CentOS 7 repository configuration file with the Aliyun mirror URL.
2. We only replace the `CentOS-Base.repo` file in the `/etc/yum.repos.d/` directory, which is the default repository configuration file for CentOS 7. Other repository configuration files will be updated automatically when we run `yum makecache`.
3. The `yum clean` command is used to clean the cache of the old mirror, and the `yum makecache` command is used to update the cache with the new mirror.
