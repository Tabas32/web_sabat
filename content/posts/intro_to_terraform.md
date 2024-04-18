+++ 
draft = false
date = 2024-04-18T13:49:36+02:00
title = "Introduction to Terraform"
description = "How to install and run Terraform with AWS"
slug = ""
authors = [ "Marian Sabat" ]
tags = [ "devops" ]
categories = []
externalLink = ""
series = []
+++

Terraform is tool for managing infrastructure. IaC or 'Infrastructure as 
a code' is process of managing you infrastrucutre with set of tools and human
readable files instead of manually setting it in configuration tools.

## Installation

To use Terraform you need to install it. All necessary infromation can be 
found at [official website](https://www.terraform.io/). On linux machines
use package manager

```
sudo pacman -S terraform
```

If everything was installed correctly you can check it with

```
terraform -help
```

Current latest version is `1.8.0`. I will be using `1.7.4`.

## Managing AWS

To use Terraform for managing AWS infrastructure you will need AWS CLI
installed.

```
sudo pacman -S aws-cli-v2
```

I will be using version `2.15.19`, you can check your version with 

```
aws --version
```

Latest version is `2.15.39`.

### Credentials for AWS

After installation before we can use AWS CLI with terraform we need to set
up credentials, that will point CLI to correct AWS account and region.

First create new IAM user.
1. Log in to your AWS account
2. In management console search for `IAM` service
3. In left menu select `users`
4. Press `Create user`
    1. Fill in user name
    2. Select 'Provide user access to the AWS Management Console'
    3. Select 'I want to create IAM user'
5. `Next`
6. Select `Attach policies directly`
    1. Under 'Permissions policies' select `AdministratorAccess`
7. `Next`
8. `Create user` (You can log in and change default password)
9. Open details of your newly create user and switch to tab `Security credentials`
10. Under 'Access keys' press `Create access key`
11. Select `CLI` and confirmation box a proccede with `Next`
12. Fill in description and `Create access key`
13. Save your new access key to some place and `Done`

Now you need to provide these credentials to your AWS CLI. You can do it by
setting up environment variables

```
export AWS_ACCESS_KEY_ID=<your access key id>
export AWS_SECRET_ACCESS_KEY=<your secret access key>
```

## Creating Terraform repository

Now when you have set up all dependencies you should be able to create basic
terraform project and create EC2 instance with it.

Create new directory
```
mkdir terraform-aws-instance
cd terraform-aws-instance
```

Create new file with name `main.tf` and write configuration as
```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }

  required_version = ">= 1.2.0"
}

provider "aws" {
  region = "eu-north-1"
}

resource "aws_instance" "app_server" {
  ami           = "ami-0d74f1e79c38f2933"
  instance_type = "t3.micro"

  tags = {
    Name = "ExampleAppServerInstance"
  }
}
```

Terraform block contains settings for terraform including required providers.
Terraform providers are by default downloaded from
[registry](https://registry.terraform.io/). Provider block is used for 
configuring specified provider, in this case it is `aws`. Resource block
serve for defining components of infrastrucuture. It can be logical like
application or physical like sevice instance. Resource blocks are specified
by resource type (`aws_instance`) and name (`app_server`). Inside of resource
block are specified arguments for configurating this resource.

After your code is written you can procede with creating actual EC2 instance.
First initialize terraform repo
```
terraform init
```

You can formate your code with `terraform fmt` and validate it with
`terraform validate`. If you only want to see what changes will be performed
by terraform you can use `terraform plan`.

To apply you configuration use
```
terraform apply
```
This will show you what actions will be performed and it will ask you for
confirmation, type `yes`. Creation of your resources can take some time.
After everything is created, you will be able to find your new instance in
AWS. So go to your management console and under EC2 instances you will be 
able to find running instace with name `ExampleAppServerInstance`.

`apply` command will create file `terraform.tfstate`, which stores IDs and
properties of your resources. It is only way how terraform can track your
infrastructure. It can contain sensitive information, so be sure to store 
it in safe way.

To check resources in your state you can use `terraform state list`.

That is all. In this article we have installed terraform, set up AWS CLI 
and used terraform to create new EC2 instance for us.
