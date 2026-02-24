---
title: Mounting Remote Drives
description: Using NFS to mount remote Storage.
published: true
date: 2024-11-20T19:49:55.000Z
tags:
editor: markdown
dateCreated: 2024-11-20T19:40:48.333Z
---

<div align="center">
    <h1> <img src ="https://cdn.jsdelivr.net/gh/selfhst/icons@main/svg/linux.svg" alt="tux the penguin logo" style="width:20%;height:20%"> </h1>
</div>


## Intro

Hello Folks, Today We're going to learn one of the cool things in Linux and that is mounting an external Network share.


## Setting up the Share

Provided you have the share already. I'll be showing here how to mount the share.

### CLI

First, you'll want to make sure you have NFS Client installed.

- **Debian/Ubuntu**:

```shell
sudo apt update
sudo apt install nfs-common
```

- **CentOS/RHEL:**

```shell
sudo yum install nfs-utils
```

Once that is done we'll need to make a Mount Point for this.

- That can be done like this

```shell
sudo mkdir -p /mnt/nfs_share
```
- Once the Mount Point is made. We can mount it

```shell
sudo mount -t nfs nfs_server_ip:/path/to/nfs_share /mnt/nfs_share
```
I'll break down these commands for you here.

- `mount -t nfs` - Issues the mount command with the type of `nfs`.
- `nfs_server_ip` - The IP of the server.
- `/path/to/nfs_share` - Path on the target server.
- `/mnt/nfs_share` - Where the Share will be mounted on the server.

Once that is done we can test to make sure the share is mounted correctly by running a

```shell
df -h | grep nfs
```
If all looks good then we have mounted the share at least for this session. But we want this to happen on every boot. That can be done by adding a simple line to the `/etc/fstab`.

```shell
echo "192.168.1.100:/export/shared /mnt/nfs_share nfs defaults 0 0" | sudo tee -a /etc/fstab
```

Running that snippet will add it to that file but please change the variables as most likely you're mount is not the exact same as the example.

After that is done go ahead and run a `tail /etc/fstab` to make sure the line has been added to the file. If you see the line you added you're all set then just issue a `mount -a` and you'll be all set. If you have any lines that you don't want re mounted make sure they are commented out or just run `sudo mount /mnt/nfs_share` to mount that specific entry.

After that is all done the mount should stay persistent even after reboots.

## Cockpit

I've shared this video here found on [YouTube](https://www.youtube.com/embed/PGCBda3Le9Y?si=ccmR-BPg5JssM-1Q) to better show how it'd done. While it is old the only difference is instead of the `+` in storage there is a hamburger menu instead.
Epilogue
With all that being said, this is how you can mount shares in Linux. I hope you learned something here, and make sure to come back for more tutorials.









