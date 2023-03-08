---
title: "Configure and Run ECR Scanning"
chapter: true
weight: 51
---

# Overview
In this section, we'll demonstrate how Snyk [scans container images directly from Amazon ECR](https://docs.snyk.io/scan-containers/image-scanning-library/ecr-image-scanning). Teams can use Snyk's Container Registry integrations to perform baseline scanning of Container Images without interrupting Developer Workflows. 

Although we have both [Automated and Manual options for configuring the ECR integration](https://docs.snyk.io/scan-containers/image-scanning-library/ecr-image-scanning/configure-integration-for-amazon-elastic-container-registry-ecr), in this workshop we'll follow the Manual process.

{{% notice tip %}}
Snyk's integration for Amazon Elastic Container Registry (ECR) is enabled between one Amazon ECR registry and one Snyk organization. To integrate with multiple registries, create a unique Snyk organization for each registry.
{{% /notice %}}

## Step 1: Copy your Snyk Organization ID
From the Snyk console, navigate to [Snyk Account Settings](https://app.snyk.io/account) and under the __General__ menu `Copy` your __Organization ID__.

![Snyk Organization ID](../images/snyk-api-token.png)

## Step 2: Create a new IAM Policy
Snyk leverages AWS IAM to authenticate to ECR. Navigate to the AWS Console for the **IAM service**, and then **Policies**. From this view, select `Create policy` and name it `AmazonEC2ContainerRegistryReadOnlyForSnyk`. Define the body as follows:

```json
{
 "Version": "2012-10-17",
 "Statement": [
  {
   "Sid": "SnykAllowPull",
   "Effect": "Allow",
   "Action": [
    "ecr:GetLifecyclePolicyPreview",
    "ecr:GetDownloadUrlForLayer",
    "ecr:BatchGetImage",
    "ecr:DescribeImages",
    "ecr:GetAuthorizationToken",
    "ecr:DescribeRepositories",
    "ecr:ListTagsForResource",
    "ecr:ListImages",
    "ecr:BatchCheckLayerAvailability",
    "ecr:GetRepositoryPolicy",
    "ecr:GetLifecyclePolicy"
   ],
   "Resource": "*"
  }
 ]
}
```

Save this new Policy.

## Step 3: Create a new IAM Role
Next, navigate to the AWS Console for the **IAM service**, and then **Roles**. From here, select `Create role` and name it `SnykServiceRole`.  This role is for the AWS EC2 service underlying ECR, and utilizes the policy we just created.

## Step 4: Harden the usability scope
The integration works when you specify your Snyk organization by ID. For the purpose of this workshop, you are expected to only have one entry, and the format of the text will be like this:

```json
{
 "Version": "2012-10-17",
 "Statement": [
  {
   "Effect": "Allow",
   "Principal": {
    "AWS":  "arn:aws:iam::198361731867:user/ecr-integration-user"
   },
   "Action": "sts:AssumeRole",
   "Condition": {
    "StringEquals": {
     "sts:ExternalId": "2530db28-bf52-489f-be17-c313c0bed5b7"
    }
   }
  }
 ]
}
```

Save this new role. Once you are finished with creating the hardened IAM Role that uses the IAM Policy, you will have access to the ARN for the role. Save this ARN, as you'll plug it into Snyk on the next step.

## Step 5: Configure the ECR integration
With the IAM Policy and Role created, you're now ready to configure the ECR Integration. Start by navigating to Snyk's Integrations page. Here we show a filtered view for "ecr".

TODO: Needs new Picture?
![Snyk ECR Integration](../images/aws-ecr-integration.png)

Clicking the ECR tile takes you to a page with two fields to fill in, and instructions on what to do.
- The first field is your region, and we'll assume `us-east-1`
- The second field is the ARN for the AWS IAM Role you created, which will have the assumed name of the form  `arn:aws:iam::YOURACCOUNTNUMBER:role/SnykServiceRole`.

Once filled in, click Connect. You'll see a banner announcing the integration is successful.

TODO: Needs new Picture?
![Snyk ECR Success](../images/aws-ecr-integration-success.png)

## Step 6: Add ECR images to Snyk
With the integration configured, you can now add your ECR images to Snyk to scan them for vulnerabilities!

TODO: Needs new Picture?
![Snyk Add Images](../images/aws-ecr-add-images.png)

In a few moments, your images will be imported as projects and you can review the results. This method of scanning is especially helpful for teams looking to gain visibility into the security of container images without interrupting developer workflows. Once the scan completes, you will see projects for both the container base image and the open-source package manifest it contains. This is handy for seeing both in the same context in order to identify the right team to remediate those issues.

TODO: Needs new Picture?
![Snyk ECR projects](../images/aws-ecr-snyk-projects.png)
