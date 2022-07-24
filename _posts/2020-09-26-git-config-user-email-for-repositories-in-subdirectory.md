---
layout: single
title: "Git config `user.email` for repositories in subdirectory"
excerpt: "Quick hint for developers working on source code from different **Git** accounts having different e-mails for them."
date: 2020-09-26 18:34:09 +0200
categories: ["Git"]
tags: ["Git", "GitHub", "Git Config", "Email"]
---

Quick hint for developers working on source code from different Git accounts having different e-mails for them. Let say we develop our own projects and we support development for two other companies. Our folder structure could look like

```
~/.gitconfig
~/PersonalProjects/
~/CompanyAbc/.gitconfig
~/CompanyMno/.gitconfig
```

Of course we have configured default config as follows

```bash
user@mymac ~/ $ git config user.name "Firstname Lastname"
user@mymac ~/ $ git config user.email "user@myprivatebox.com"
user@mymac ~/ $ git config push.default simple
user@mymac ~/ $ git config core.editor emacs
```

We want different e-mails by default for all repositories in these directories without configuring this manually with command

```bash
user@mymac ~/CompanyAbc/website-repo (master) $ git config user.email "user@company-abc.com"
```

every time when wy clone new repository.

## Solution

Main `~/.gitconfig` file should look like

```config
[user]
    name = Firstname Lastname
    email = user@myprivatebox.com
[push]
    default = simple
[core]
    editor = emacs
[includeIf "gitdir:~/CompanyAbc/"]
    path = ~/CompanyAbc/.gitconfig
[includeIf "gitdir:~/CompanyMno/"]
    path = ~/CompanyMno/.gitconfig
```

Set custom e-mail address in `~/CompanyAbc/.gitconfig`

```config
[user]
    email = user@company-abc.com
```

You can repeat this for other configurations like `~/CompanyMno/.gitconfig` too.

## Verification

Please execute command to verify new settings

```bash
user@mymac ~/CompanyAbc/some-repo/ (master) $ git config user.email
```

It should throw out something like

```
user@company-abc.com
```

Good luck with coding!
