---
title: "Day 4"
date: 2023-02-05T23:12:00+01:00
---

## Content of the day

No lessons for today, but two things:

* creation of a docker image with `packer` and `aws-cli`
* testing this image with a base `packer.json` file to test simple (no-provisioning) AMI creation

### docker packer aws

https://github.com/leddzip/docker-packer-aws

This is based on a previous work docker with terraform and aws cli.
Since both terraform and packer are from the same provider,
it's quite easy to port one work to the other

To use it, simply run this command:

```bash
docker run --rm -e AWS_ACCESS_KEY=XXX -e AWS_SECRET_ACCESS_KEY=YYY -v $(pwd):/workdir leddzip/bash-packer-aws:latest packer build packer.json
```

where `XXX` and `YYY` should be replaced accordingly.

### Base ami and test

The previous image has been tested using the following `packer.json`:

```json
{
  "builders": [
    {
      "type": "amazon-ebs",
      "region": "us-east-1",
      "instance_type": "t2.micro",
      "ami_name": "packer-base-ami-{{timestamp}}",
      "source_ami_filter": {
        "filters": {
          "virtualization-type": "hvm",
          "name": "ubuntu/images/*ubuntu-focal-20.04-amd64-server-*",
          "root-device-type": "ebs"
        },
        "owners": ["099720109477"],
        "most_recent": true
      },
      "ssh_username": "ubuntu"
    }
  ]
}
```

and the command described in the previous section.
Tested using the credential for an AWS sandbox,
I can see a new AMI created (` packer-base-ami-1675633736`).

Since I inject the credential using environment variable,
I don't need anymore the block 

```json
{ ...
  "variables": {
    "aws_access_key": "",
    "aws_secret_key": ""
  },
  ...
}
```

as well as the builder entry: 

```json
{
  "access_key": "{{user `aws_access_key`}}",
  "secret_key": "{{user `aws_secret_key`}}"
}
```

### next steps

The next step should be related to provisioning
and automations with github actions.