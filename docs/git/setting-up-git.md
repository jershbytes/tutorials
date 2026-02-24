---
title: Time 2 Git Gud
description: A Gudde on how to setup git.
published: true
date: 2024-11-20T19:01:22.465Z
tags: windows, linux, git, code, version-control
editor: markdown
dateCreated: 2024-11-20T18:06:00.901Z
---

<div align="center">
    <h1> <img src ="https://cdn.jsdelivr.net/gh/selfhst/icons@main/svg/git.svg" alt="git logo" style="width:20%;height:20%"> </h1>
    <h3> Time to Git Gud </h3>
</div>


## Intro

Hello Ladies and Germs, To my class on how to **`Git Gud`**. I'll be showing you how to install git on your machines and also adding ssh keys and stuff.

<br>

## Installing Git

Installing Git is easy I'll be showing you how to on **Windows** and **Ubuntu** here.



###  Windows
```pwsh
winget install git.git
```

### Ubuntu

```shell
sudo apt install git
```
<br>

## Configuring Git

This part can be weird at times but luckily for you, I have scripted this part.

### Windows
```pwsh
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/JershBytes/labDocsrefs/heads/main/Git/scripts/git-config.ps1" -OutFile "git-config.ps1"
```
### Linux (curl is os agnostic)
```shell
curl -o git-config.sh "https://raw.githubusercontent.com/JershBytes/labDocs/refs/heads/main/Git/scripts/git-config.sh"
```

Once you have the files down for the system you're running just run the scipt by doing a

- `./git-config.ps1`

or

- `./git-config.sh`

This will ask for the email and username you want to use on commits for [**GitHub**](https://github.com/). Along with settings I think are the best for using git.

## Adding SSH Keys

SSH Keys are a key part of using Git in my opinion. Make it easier to clone and some Sites like **GitHub**** do not allow pushing via HTTPS.

Lucky for us creating an SSH key is the same on every platform. Below I'll show you how this is done.

```shell
cd .ssh
ssh-keygen -t ed25519 -C "GitHub"
```

I'll explain the bits here.

- `ssh-keygen`: The Command to generate the key.
- `-t ed25519`: The type of key to be generated in this case **ed25519**.
- `-C "GitHub"` This is a comment to be added to be the key, It is optional but helps me with what the keys are besides the name.

After that command is run it will ask what you want the name of the file to be. In this case, just type GitHub to make it easier. After that key is generated we need to add something to the ssh config but also add this key to GitHub.

```
Host github.com
        HostName github.com
        User git
        IdentityFile ~/.ssh/keys/github/GitHub
        IdentitiesOnly=yes
```
This will make it so when you're doing git commits with SSH that it only uses that key and ignores the rest of the SSH config. I suggest this go at the top of the config.

Once that is done follow this [Guide](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account) to add the key to your account. After that, we can test it with this command.

```shell
ssh -T git@github.com
```

> Hi coloredbytes! You've successfully authenticated, but GitHub does not provide shell access.
{.is-info}

If configured correctly you should get some output like this. If so congrats git is configured and we can start doing some fun stuff.







