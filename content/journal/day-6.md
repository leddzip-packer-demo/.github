---
title: "Day 6"
date: 2023-02-07T22:32:40+01:00
---

## Content of the day

Today was the creation of the base infra for a new terraform plan using the generated AMI.

But to do so, I create a dedicated repo to manage all the base infra creation with, for now, the
S3 creation and configuration and the DynamoDB table that will be used for the terraform backend of
the future plans.

I also took the opportunity to update the docker terraform-aws image ith ubuntu 22.04, awscli2 2.9.21 and 
terraform 1.3.7.

### Terraform Backend

Why ? Because, if not specified, terraform will store its state, for future reference and know what update,
destroy or creation is needed, in the current directory (locally).

In a CI/CD setup, or simply a setup where multiple developers could run a `terraform plan|apply|destroy`, we need
to store the terraform state in a remote place that will ve accessible by everybody (so terraform, even locally
still understand the state of the current infra/deployment).

One widespread way to do so is using a S3 bucket to store the `terraform.tfstate` file as well as a `DynamoDB` table
to manage the lock on this file (to avoid concurrent access to this file).

The terraform that will create this setup is here: https://github.com/leddzip-packer-demo/terraform-base-infra

Since I am using a short lived sandbox to play around, I choose to create this element while keeping the `tfstate` file
locally. If I were in a real company setup, I'd

1. run this plan locally to have the bucket and table created
2. move the state to the remote bucket in the correct location
3. update this plan to use the create s3 bucket and dynamodb table as the backend (while targeting the correct state file)

But this is an hassle in my current setup, so I choose not to do it.
However the deployments of a new EC2 instance using the created AMI will use the newly created S3 backend
since, for them, the bucket will already be in place (I will take care of recreating my infra before experimenting).

Note to myself. I run into a 403 Access Denied error while creating the bucket, but it was not related to any
specific credential issue. I put a `object_lock_enabled = true` to ease the lock, and following the advice on
https://registry.terraform.io/providers/hashicorp/aws/3.75.1/docs/resources/s3_bucket_object_lock_configuration#object-lock-configuration-for-an-existing-bucket
to put it directly in the S3 bucket block since it is for a newly created bucket. Just removing doesn't impact
a lot this backend configuration and usability but remove this 403.

Another note. To debug what is happening in terraform, and since I am using it inside a docker container, there
is an env variable `TF_LOG=DEBUG` that can be setup and provided to the docker container. It this env variable is
setup, terraform will log every thing that is happening. Very useful to debug weird behaviour like the previous 403.