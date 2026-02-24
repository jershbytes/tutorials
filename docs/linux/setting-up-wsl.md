---
title: Setting Up WSL
description: Setting up a Linux dev environment in Windows.
published: true
date: 2024-11-20T19:47:39.806Z
tags:
editor: markdown
dateCreated: 2024-11-20T19:40:56.845Z
---

<div align="center">
    <h1> <img src ="https://cdn.jsdelivr.net/gh/selfhst/icons@main/svg/windows-terminal.svg" alt="windows-terminal" style="width:20%;height:50%"> </h1>
</div>

## Install WSL and its Dependencies.

```pwsh
wsl --install --no-distribution
```
- This is the only step for Installing WSL and its dependencies.

## Windows Configuration

### Set WSL 2 as your default version
```pwsh
wsl --set-default-version 2
```
!!! note
    You may need to download the WSL2 Kernel update. This can be found [here](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi). Afer that is installed just run the command above and then follow the guide from there.


### Create .wslconfig file
The next step is to create a `.wslconfig` file so you can add more control over WSL.

- In a `Powershell` shell. Copy That and boom you should be good.

```pwsh
@"
[wsl2]
memory = 4G

[experimental]
autoMemoryReclaim = gradual
sparseVhd = true
"@ | Out-File -FilePath "$env:USERPROFILE\.wslconfig" -Encoding utf8
```

This gets posted on your Windows PC. Below I'll break down what they do.

- **autoMemoryReclaim**
  -   This setting is used to control the automatic reclamation of memory from WSL instances.
  - **`gradual`**: Specifies that memory should be reclaimed gradually, which helps in preventing performance issues due to sudden memory reclamation.
- **sparseVhd**
  -  This setting controls whether the virtual hard disks (VHDs) used by WSL instances are created as sparse files.
  -  **`true`**: Specifies that the VHD should be created as a sparse file.

## Installing **Ubuntu 24.04**.

- In a Powershell Shell, we will need to copy that command to install Ubuntu 24.04.

```pwsh
wsl --install -d Ubuntu-24.04
```
```
user: devops
pass: somdevops
```

> User and Pass do not need to be that this is just an example of when it asks you.
{.is-info}


## Configuration of Ubuntu

After You're dropped into Ubuntu. We're going to do some tweaks to the Quality of life and make WSL a little more fun.

> These next steps should be performed as root. That can be achieved by using the command below.
{.is-info}


```bash
sudo -i
```
### Update Script

- Go ahead and copy this into the terminal.

```bash
touch /usr/local/bin/update && chmod +x /usr/local/bin/update
cat <<EOF >/usr/local/bin/update
sudo apt update
sudo apt upgrade
EOF
```
What this does is drops that little upgrade script into `/usr/local/bin` which is specifically used to store executable files (binaries) that are intended to be accessible by all users on the system but are not part of the core operating system. This Specific script just does a system update with one command instead of two.

### WSL config file

```bash
touch /etc/wsl.conf
cat <<EOF >/etc/wsl.conf
[boot]
systemd=true
[automount]
root = /
options = "metadata,dmask=022,fmask=033"
[user]
default=devops
EOF
```

This config file just setups wsl to act more like a native Linux system. But Also specifies the default user.

# Epilogue

Well, Ladies and Gents. This is the end. In this post, we Installed WSL and its dependencies. Installed Ubuntu and made some tweaks. You now have a work Linux env inside Windows and you can do a whole lot with it. So go out there and have some fun.





