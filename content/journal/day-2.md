---
title: "Day 2"
date: 2023-02-01T22:56:52+01:00
---

## Content of the day

Learning packer.

[reference commit](https://github.com/leddzip-packer-demo/packer-ami-creation/commit/07ea372e82c25daeb391e96aa01e0453837e3e0c)

For now, I follow along one tuto. It starts by defining a *builder*, the base setup and configuration
needed for packer to build the ami that I want to create. The content of this builder is mostly
to select what will be the base images, the instance that will run the base image to modify and some
connection setup.

### format

for now, the tuto is using json, I will change it to hcl very soon since I don't like using json for such
configuration. Surprisingly, after naming the file `packer.json`, intellij offer a support to help me
fill up some elements.

### builder section

* `type`: here `amazon-ebs`. Since it will be used to create AMI backed by Elastic Block Storage volumes, so
   it can be used by ec2
* `access_key` and `private_key`: will receive the variable defined prior the builder block. It uses a kind of interpolation with {{ }}
* `region`: the AWS region that we will use
* `instance_type`: here, `micro.t2`, no need for more yet. The base AMI that we will use will be launched with
   this instance type. Unless we are doing so compilation on the fly to install some package, we won't need more
* `ami-name`: the name of the ami we will be creating
* `source_ami_filter`: this one is tricky. I was expecting a cold hard ref to the ami we will use like `ami-0123456789`.
   However, the tuto guide us in using a kind of filtering. Basically, those are the filter you can see when using
   directly the AWS AMI interface to search for AMI. Here, we give specific filters to target the **latest** (`most_recent: true`)
   base AMI **from Canonical** (`owners: 099720109477`), that have a **name** looking like `ubuntu/images/*ubuntu-focal-20.04-amd64-server-*`.
   There are additional filters like the kind of virtualization (`hvm`) and the root device type (`ebs`). Not sure however 
   what they are used for (except filtering of course).