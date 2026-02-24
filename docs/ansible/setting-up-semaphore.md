---
title: Deploying Ansible Semaphore
description: A step-by-step guide to deploying Ansible Semaphore for infrastructure automation.
published: true
date: 2024-11-20T18:41:18.597Z
tags: ansible, automation, linux, windows, code, IaC
editor: markdown
dateCreated: 2024-11-20T18:06:00.901Z
---


<div align="center">
    <h1> <img src ="https://cdn.jsdelivr.net/gh/selfhst/icons@main/svg/semaphore-ui.svg" alt="ansible logo" style="width:20%;height:50%"> </h1>
</div>

## Intro

Are you tired of manually remoting into your servers and updating them? Well today I have a treat for you. We're going to set up Ansible but with a GUI. Because everyone loves GUI's.

## Setting up Semaphore

We're going to use the docker approach here. As It's quick and easy and can be easily managed and updated.

### Installing Docker

You can follow the docs [here](https://docs.docker.com/engine/install/) to install docker.

### Installing Semaphore

```yaml
---
version: '3'
services:
  semaphore:
    image: semaphoreui/semaphore:v2.10.22
    container_name: semaphore
    hostname: semaphore
    ports:
      - "3000:3000"
    environment:
      - SEMAPHORE_DB_DIALECT=bolt
      - SEMAPHORE_ADMIN=admin
      - SEMAPHORE_ADMIN_PASSWORD=changeme
      - SEMAPHORE_ADMIN_NAME=Admin
      - SEMAPHORE_ADMIN_EMAIL=admin@localhost
    volumes:
      - semaphore_data:/var/lib/semaphore
      - semaphore_config:/etc/semaphore
      - tmp_config:/tmp/semaphore
    restart: unless-stopped

volumes:
  semaphore_data:
  semaphore_config:
  tmp_config:
```

However you spin up docker containers. Whether it's with `docker-compose` from the cli or using portainer to set up the stack itself once that is all done you should be able to access the application at `IP:3000`.

## Configuring Semaphore

### How Ansible Works.

At its core Ansible is agent-less, so it needs no agent, just needs a way to connect to the server. In this case we just need some Good ole SSH Keys to connect to the server. Since Semaphore uses its own SSH Server we need to don't need it on the system unless you want to do it that way.

### Adding SSH Keys

This should be self-explanatory but because I like you ill go over this again.

```shell
cd .ssh
ssh-keygen -t ed25519 -C "Semaphore"
```

I'll explain the bits here.

- `ssh-keygen`: The Command to generate the key.
- `-t ed25519`: The type of key to be generated in this case **ed25519**.
- `-C "GitHub"` This is a comment to be added to be the key, It is optional but helps me with what the keys are besides the name.

Once the key is created and named all you need to do is add it to semaphore.

That can be done by following these steps.

`Key Store --> New Key --> SSH Key from dropdown.`

Name the key and paste the private key in the Dialog Box.

After that make sure the public key is in the `Authorized_keys` file on the target machine.

### Building an Inventory

We can test this by using a playbook. But first we need to built on an inventory.

For us to be able to do that follow the steps below.

`Inventory --> NEW INVENTORY --> Select Ansible --> Select Static

```ini
[linux servers]
172.18.8.40
```
File should look something like that.

Once that is done we're close to be being able to test. We just need a playbook.

### Configuring Repo for Task Templates.

Before we get to adding the Task we need a repo to test it on.

Let's use this repo [here](https://github.com/ansible/test-playbooks.git)

Ansible's own test playbooks.

To add it follow these steps.

- Click Repositories
- Click NEW REPOSITORY
  - Name it
  - **https://github.com/ansible/test-playbooks.git** (add that as URL)
  - Branch is main
  - No key is required as this it `HTTPS` so selected none you may need to create a non key in the key store which is easy

**Adding None Key if needed**

- Click Key Store
- Click New Key
  - Select None from drop down.
  - Name it none. (You're done)

### Adding Task Template

Almost Done Here folks, now we add the Task.

- Click Task Templates on Side bar
- Click NEW TEMPLATE
- Select Ansible playbook
  - Name it ping
  - Description is not needed
  - File name is going to be `ping.yml`
  - Inventory is the one you created
  - repo is the one we just created as well
  - env needs to be set that. ( go ahead and make an empty one if not already.)

After that is all done you should be good to test the playbook if all the keys were added correctly. The playbook should be finished with a Success and a green okay in the window.

## Epilogue

Well that is all for today folks, We set up Ansible semaphore and tested a playbook to verify all is working good. If you have any questions or something is not clear feel free to open an [Issue](https://github.com/coloredbytes/learninghub/issues).






