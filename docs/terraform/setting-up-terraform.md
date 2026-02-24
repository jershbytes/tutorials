---
title: Github As Code
description: Using Terraform with Github.
published: true
date: 2024-11-20T20:20:21.185Z
tags: linux, window, code, automation, IaC, terraform, github
editor: markdown
dateCreated: 2024-11-20T19:22:22.121Z
---

<div align="center">
    <h1> <img src ="https://cdn.jsdelivr.net/gh/selfhst/icons@main/svg/hashicorp-terraform.svg" alt="terraform logo" style="width:20%;height:50%"> </h1>
</div>


## Installing Terraform

### Windows
```pwsh
winget install Hashicorp.Terraform
```

### MacOSX/Linux

```bash
brew tap hashicorp/tap
brew install hashicorp/tap/terraform
```

### Ubuntu/Debian

```bash
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform
```

### CentOS/RHEL

```bash
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
sudo yum -y install terraform
```



## Setting up your workspace.

I like to create a `terraform` folder in my home directory or somewhere where I'll remember.

- Following the commands below I'll first show you how to do this on `Linux` and then `Windows`.

### Linux

```bash
cd ~
mkdir terraform;cd terraform
touch {provider,main,variables}.tf
```

### Windows

```pwsh
cd ~
mkdir terraform;cd terraform

# Next Part is a for loop to create the files from above put on windows.

foreach ($name in "provider.tf", "main.tf", "variables.tf") {
    New-Item -Path . -Name $name -ItemType "file"
}
```



## Utilizing Terraform

Now, That we have all that setup we need a project. I Use Proxmxox in my lab put some of my friends use Hyper-V. So Let's try Making a GitHub Reepo.

### The `provider.tf` file

- In your Text Editor of choice open the `provider.tf` file and add this bit to it.

```tf
terraform {
  required_providers {
    github = {
      source  = "integrations/github"
      version = "~> 6.0"
    }
  }
}

# Configure the GitHub Provider
provider "github" {
      token = var.token # or `GITHUB_TOKEN`
}
```

### The `main.tf` file

- In your Text Editor of choice open the `main.tf` file and add this bit to it.

```tf
resource "github_repository" "repo" {
  name        = var.repo_name
  description = "My awesome codebase"

  visibility = "public"
}
```

### The `variables.tf` file

- In your Text Editor of choice open the `variables.tf` file and add this bit to it.

```tf
variable "token" {
    type = string
    description = "GitHub PAT Token"
}

variable "repo_name" {
    type = string
    description = "Name of the repo"
}
```

<br>

Now that we have most of that setup. We're almost ready to try this bad-boi out.

The best way to separate this is instead of putting the variables in plain text in case you make this a git repo. is make yet another file :smile:.

### The `terraform.tfvars` file

- In your Text Editor of choice open the `terraform.tfvars` file and add this bit to it.

```tf
repo_name = "value"
token = "value"
```

With that being done it's not in plain text and if you decided to version control your terraform. You can just add a `.gitignore` file to ignore that from being pushed.

## Testing Your Work

We're almost done here we just need to do some things before the magic happens.

- Run these three commands in your shell.

```bash
terraform init
terraform plan
terraform apply
```
Here is a breakdown of these commands.
- `terraform init` - Initializes the Provider.
- `terraform plan ` - Buts the plan or the files into staging.
- `terraform apply ` - Applys the work, In this case creating the repo.

After this is all done check your GitHub to see if the new repo is there. If it's there congrats you did it right. If not I have failed you as a teacher.

***sad capybara noises***

## Epilogue

With all this being said I hope you had fun doing this. I know it was something cool for me to learn. Till next time ladies and gents.


- **If you don't have a PAT token for github and don't know how to set it up. Check out the video [here](https://www.youtube.com/embed/IuiH6cBtc58?si=q7n5ws0Y_MBETm5Z) .**

