---
layout: post
title: "[Glean] Debugging Git Using Trace"
description: "Debugging Git Using GIT_TRACE and restart the gpg-agent to solve the gpg failed to sign the data."
categories: [Glean]
tags: [Git, Trace]
last_updated: 2021-01-08 11:58:00 GMT+8
excerpt: "Debugging Git Using GIT_TRACE and restart the gpg-agent to solve the gpg failed to sign the data."
redirect_from:
  - /2021/01/08/
---

* Kramdown table of contents
{:toc .toc}
# Debugging Git Using Trace

We can debugging and get to know the command that Git executed by `GIT_TRACE`.

For example, when I meet the problem related to GPG signature when I commit:

```
error: gpg failed to sign the data
fatal: failed to write commit object
```

Then I can use this command to check the problem:

```bash
GIT_TRACE=true git commit
```

Then I can see:

```
..... trace: run_command: gpg --status-fd=2 -bsau xxxxxxxxx
```

After execute this command manually, it had no response.

Therefore, I killed the `gpg-agent` and restart it again.

```bash
gpgconf --kill gpg-agent
```

