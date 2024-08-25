---
layout: post
title: "[Tutorial] Schedule Command for CentOS using `crontab`"
description: "Schedule command for CentOS using `crontab`, including crontab syntax, examples, output, and environment variables."
categories: [Tutorial]
tags: [Linux, CentOS, Crontab]
last_updated: 2024-07-31 11:51:00 GMT+8
excerpt: "Schedule command for CentOS using `crontab`, including crontab syntax, examples, output, and environment variables."
redirect_from:
  - /2024/07/31/
---

* Kramdown table of contents
{:toc .toc}

# Schedule Command for CentOS using `crontab`

## Introduction

`crontab` is a command-line utility that allows you to schedule tasks to run at specific times or intervals. You can use `crontab` to run scripts, commands, or other programs automatically at a specified time.

## Install and Enable `crontab`

```bash
# Install `crontab`
sudo yum install cronie
# Start and enable `crontab`
sudo systemctl start crond
sudo systemctl enable crond
```

Then, check the status of `crond`:

```bash
sudo systemctl status crond
```

## Start and Edit Crontab Schedule

```bash
crontab -e
```

After editing the crontab file, save and exit the editor. The cron daemon will automatically reload the crontab file with the following message:

```plaintext
crontab: installing new crontab
```

## Crontab Syntax

```plaintext
# ┌───────────── minute (0 - 59)
# │ ┌───────────── hour (0 - 23)
# │ │ ┌───────────── day of month (1 - 31)
# │ │ │ ┌───────────── month (1 - 12)
# │ │ │ │ ┌───────────── day of week (0 - 6) (Sunday to Saturday)
# │ │ │ │ │
# │ │ │ │ │
# * * * * * command to execute
```

## Crontab Examples

```bash
# Run a command every minute
* * * * * /path/to/command

# Run a command every 5 minutes
*/5 * * * * /path/to/command

# Run a command every hour
0 * * * * /path/to/command

# Run a command every day at 3:30 AM
30 3 * * * /path/to/command

# Run a command every week on Sunday at 3:30 AM

30 3 * * 0 /path/to/command

# Run a command every month on the 1st at 3:30 AM
30 3 1 * * /path/to/command
```

## Crontab Examples with Output

```bash
# Redirect output to a file
* * * * * /path/to/command > /path/to/output.log

# Redirect output to a file and append
* * * * * /path/to/command >> /path/to/output.log

# Redirect output to a file and error to another file
* * * * * /path/to/command > /path/to/output.log 2> /path/to/error.log

# Redirect output and error to the same file
* * * * * /path/to/command > /path/to/output.log 2>&1
```

## Crontab Examples with Environment Variables

```bash
# Set environment variables
* * * * * export VAR=value; /path/to/command

# Set environment variables from a file
* * * * * source /path/to/env.file; /path/to/command
```

## Check Crontab Execution Result

You can check your email for the output of the crontab command when you see the following message:

```plaintext
You have new mail in /var/spool/mail/xxx
```

You can check the mail content by using the following command:

```bash
mail
# or
cat /var/spool/mail/xxx
```
