---
title: "Create BitBucket Repo Variables"
chapter: true
weight: 44 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES IF APPLICABLE
---

# Overview
With the Bitbucket Repo created and cloned to your Cloud9 IDE, the next step is to create the [repository variables](https://support.atlassian.com/bitbucket-cloud/docs/variables-in-pipelines/#Repository-variables) used by Bitbucket Pipelines. When the pipeline is triggered, these variables will be [referenced by your pipeline](https://support.atlassian.com/bitbucket-cloud/docs/variables-in-pipelines/) to connect to your AWS ECR and Snyk instances.

## Configure Repository variables
Create the following variables as shown below, using the variable names provided:

1. Snyk API token for authenticating with Snyk: `SNYK_TOKEN`.  This is obtained from the [Snyk Account Settings](https://app.snyk.io/account) page.
1. AWS Identity & Access Management User [key and secret](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) for secure authenticated interactions with the AWS API: `AWS_ACCESS_KEY_ID` & `AWS_SECRET_ACCESS_KEY`
1. AWS region you will be deploying to: `AWS_DEFAULT_REGION` with the value of `us-east-1`
1. Container image name: `IMAGE` with the value of `goof`.  This needs to match the name of your ECR Repository.
1. The URI to your AWS ECR: `AWS_ECR_URI`.  We use this for the deployment to EKS.
1. (OPTIONAL) Amazon EKS name of your cluster: `AWS_EKS_CLUSTER` if you have configured it in a previous step.

This screenshot shows those repository variables:
![Repository Variables](/images/bitbucket-repo-vars.png)

{{% notice tip %}}
To authenticate with Snyk, we are using Snyk API Tokens for this workshop. In Production, it is recommended that you use [Service accounts](https://support.snyk.io/hc/en-us/articles/360004037597-Service-accounts). Also be sure to consider [AWS IAM best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) when creating accounts for CI/CD pipelines.
{{% /notice %}}