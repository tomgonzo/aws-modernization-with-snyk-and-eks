---
title: "Create ECR Repository"
chapter: true
weight: 24 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES IF APPLICABLE
---

# Overview
In this workshop, you'll use Bitbucket Pipelines to build and containerize a sample application then push that container image to Amazon ECR. This page will guide you through [creating a repository](https://docs.aws.amazon.com/AmazonECR/latest/userguide/repository-create.html) to store your container images.

## Create the ECR Repo from the AWS Console
Amazon ECR repositories can be created from the AWS Management Console, or with the AWS CLI and AWS SDKs. For this workshop, we will create the repository from the AWS Management Console:

1. Open the Amazon ECR console at https://console.aws.amazon.com/ecr/repositories.
1. From the navigation bar, select `us-east-1` as the active Region.
1. In the ECR navigation pane, choose Repositories.
1. On the Repositories page, choose Create repository.
![Create ECR](/images/aws-ecr-create-repository.png)

1. Use the defaults for Private repository, and enter a unique name for your repository.  For the purpose of this workshop, we'll call it `goof`
1. For Tag immutability, choose the tag mutability setting for the repository. Repositories configured with immutable tags will prevent image tags from being overwritten. For more information, see Image tag mutability.
1. For Scan on push, choose the image scanning setting for the repository. Repositories configured to scan on push will start an image scan whenever an image is pushed, otherwise image scans need to be started manually. For more information, see Image scanning.
![Create ECR](/images/aws-ecr-create-repository-details.png)

1. Choose Create repository.

Make note of the URI for your repository, which will be in this form:

`478468688580.dkr.ecr.us-east-1.amazonaws.com/goof`

