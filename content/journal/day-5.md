---
title: "Day 5"
date: 2023-02-06T23:29:20+01:00
---

## Content of the day

https://github.com/leddzip-packer-demo/packer-ami-creation

This is the kinda final mandatory step required for my use case.
Basically, being able to provision the AMI so it have the application that I need.

I am limiting myself (for the exercise) in installing `docker` and `docker-compose` as well as
uploading one file/folder to a specific location in the new AMI. However, the exercises show case 
how to create a base AMI image to run an application (using `systemctl` to register and start a service).

I don't think we will have such use case in the future, but it is quite easy to image having to start an
EC2 with java, tomcat and httpd. Who knows ?

### Provisioning

A provisioning block should at least have a `type` field.
There is two kind of provisioning type that I am interested in:

* shell
* file

Others exist such as `chef`, `ansible`, `salt`, `puppet`, ... but I won't use it (I don't have the use case 
for this).

The `shell` provisioning can be either `inline` or target specific scripts that will be run on the target.
Additional argument can be added such as `environment_vars` and options to keep the scripts on the remote machine,
or to specify where the script will be upload and run.

The `file` take a `source` which is in the current folder structure, and a destination which is on the AMI we want
to create. Just a little note:

```json
{
  "provisioners": [
    {
      "type": "file",
      "source": "scripts/",
      "destination": "/tmp"
    }
  ]   
}
```

will copy the whole content of `scripts` and put it in `/tmp` such as `/tmp/file1` for example.

However, if we don't put a / at the end of the source, `"source": "sripts"`, it will create an intermediary
folder in the target: `/tmp/scripts/file1`.