---
date: 2021-02-21 03:46:12
layout: post
title: How To Configure SSH Key-Based Authentication
subtitle: Secure your ssh server by restricting authentication to ssh-keys
description: Secure your ssh server by restricting authentication to ssh-keys
image: /assets/img/uploads/ssh-keys.jpg
optimized_image: /assets/img/uploads/ssh-keys.jpg
category: blog
tags:
  - unix
author: jaimedearcos
paginate: false
---
> Recently I had to set-up a local server for some side projects I'm working at. I had configured ssh server to remote access, but I was afraid about the security risks of having external access to my home network with password login so I restricted the authentication to only ssh keys.

## 1. Generate ssh keys

```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/my-server.key -C "My server key"
```

> Is strongly recommeded to use also a strong passphrase to avoid brute force attacks

This command will generate 2 files with 4096 bits RSA key with a comment:

* `~/.ssh/my-server.key` – private key.
* `~/.ssh/my-server.key.pub` – public key.

## 2.Copy public key in the server

Just execute:

```bash
ssh-copy-id -i ~/.ssh/my-server.key.pub your-user@my.server.com
```

This is the same as  copy the public key file to `~/.ssh/authorized_keys` directory of the server

Now you should be able to log in your server without the password

## 3. Disable password login on the server

Just edit the file `/etc/ssh/sshd_config` (in server), search for the following fields and set to no

```
PermitRootLogin no 
ChallengeResponseAuthentication no
PasswordAuthentication no
UsePAM no
```

 Restart the sshd server with:

```bash
sudo /etc/init.d/ssh reload
```

### 4. Correct  `.ssh`  directory  permissions

Another security step for your ssh server is setting the right/minimum permissions in the `.ssh` folder.

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
`` 
