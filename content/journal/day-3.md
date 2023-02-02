---
title: "Day 3"
date: 2023-02-02T22:33:49+01:00
---

## Content of the day

This lesson is on `communicator`.

Since I am using linux, or at least *nix based system, I don't need to provide a `communicator` in the
`builder` section. However, I do have to provide some additional information so that packer can `ssh` inside the
`ec2` that will hold the base AMI.

* `ssh_username`: here will be `ubuntu`. Depends on the base AMI used. Should refer to the doc of the AMI used

to validate the packer file (works only with json file for now, does not work with hcl files): `packer validate packer.json`.

### regarding credentials (`aws_access_key` and `aws_secret_key`)

for aws, we don't have to provide them manually (and define them inside the block variable).
As long as you have a `aws-cli` setup with your credential (be careful if you are using multiple accounts),
it should be automatic (`.aws/credential`).

If you don't want to use your default profile, you will have to define the entry `profile` inside your builder
to tell `aws-cli` what is the profile you want to use => that means that, for company setup, with agent configured
correctly, one should not be concerned with providing by themselves those credentials.

good refs on the subject: 

* https://cloudcasts.io/course/a-packer-primer/introduction
* https://www.youtube.com/watch?v=zF58Z4TdlrA

That does mean however that I should find or create a docker image with packer and aws cli if I want to 
make sure I won't be using my own account, and if I want my packer to be run inside a CI/CD tools (like github 
action or circleCI). 

I think I will anyway build a new Docker image, just like I did in the past for terraform+aws-cli: https://github.com/leddzip/docker-terraform-aws
In the end, the command for terraform was:
```bash
 docker run -it --rm --name terraofrm-test -e AWS_ACCESS_KEY_ID="XXX" -e AWS_SECRET_ACCESS_KEY="YYY" -v "$(pwd):/workdir" leddzip/bash-terraform-aws:local-0.1.1
```

so I expect something similar for packer. Here, having the environment variable `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`
defined is enough for `aws-cli` (used under the hood by terraform (and packer I assume)) to connect to the correct account.