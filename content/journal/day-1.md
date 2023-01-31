---
title: "Day 1"
date: 2023-01-31T21:22:25+01:00
---

The experiment is to work with [packer](https://www.packer.io/) to create a bew base AMI.

## What I have in hand:

* a bit of experience with [terraform](https://www.terraform.io/) and [HCL](https://github.com/hashicorp/hcl)
* a bit of experience with AWS
* some practical experience with devops practices

## What am I targeting:

* a fully functional AMI with docker and docker-compose installed, build with packer
* once the AMI is created, use it to create an EC2 with terraform
* all build should be done through github action (I use heavily CircleCI in most of my projects, so let's try something else once)
* a documentation of my progress using hugo (for fun and to learn as muck as possible along the way)

## Content of the day;

Mostly setup of the projects and the different repos to hold the work on this experiment.
Currently there are 3 repos:

* [one for packer](https://github.com/leddzip-packer-demo/packer-ami-creation) based AMI creation
* [one for terraform](https://github.com/leddzip-packer-demo/terraform-using-ami-ec2) using the create AMI to bootstrap an EC2 instance
* [this site](https://github.com/leddzip-packer-demo/leddzip-packer-demo.github.io), using hugo, to document and log my journey

Most of the repo are empty for now, excpet the one for my dev logging.
The work done on it is:

* setup of hugo locally
* setup of gh action to deploy this site into a gh-page
* theme configuration and content creation

## Some thoughs

I am pretty sure I'll need a docker image with packer if I want to use it easilly. I could use it directly with a ubuntu runner and install packer in it, but it won't fit the workflow I use with CircleCI (additionnally isolating the build env offers many benefits).

For now, hugo is fairly easy to use. A little derouting at the beginning (especially due to the theme I choosed), but once setup, it's a very nice tool.

## References for further experimentation

* https://stackoverflow.com/questions/57549439/how-do-i-use-docker-with-github-actions
