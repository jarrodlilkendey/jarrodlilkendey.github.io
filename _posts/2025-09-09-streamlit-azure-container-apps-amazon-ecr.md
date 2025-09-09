---
title: "Deploy Streamlit to Azure Container Apps with IAC and CICD - Provision a Public Container Repository with Amazon ECR"
layout: post
date: "2025-09-09 12:00:00 +1000"
permalink: posts/streamlit-azure-container-apps-amazon-ecr/
categories: [Streamlit to Azure Container Apps with IAC and CICD]
tags:
  [
    streamlit,
    azure,
    azure-container-apps,
    terraform,
    github-actions,
    cicd,
    iac,
    cloud,
    docker,
  ]
# img_path: /assets/images/position-images-next-to-div/
---

In this post I will go over the provisioning of a public container image repository inside an Amazon Elastic Container Registry using Terraform as well as pushing our Docker container image into ECR.

This post is part 4 of a [series](/categories/streamlit-to-azure-container-apps-with-iac-and-cicd/) of posts where I will write about my solution for getting a streamlit application deployed to Azure Container Apps with IAC and CICD.

The code repository for this tutorial is available on my GitHub repo [streamlit-azure-container-app](https://github.com/jarrodlilkendey/streamlit-azure-container-app).

## Amazon Elastic Container Registry

In the previous part of this series we created a Docker container image of the streamlit application. So how do we take this build artifact from our local machine and put it into a location we can use to deploy it into the cloud?

### What is a Container Registry?

There is where container registries come into the picture, container registries store the container images we build either locally or on a continuous integration server.

Some examples of well known container registries are DockerHub, Azure Container Registry and Amazon Elastric Container Registry.

### Why did I select Amazon Elastic Container Registry?

For me it came down to price. Azure Container Registry is a little pricier than Amazon Elastric Container Registry, so I picked the cheaper option.

### Why use a public registry?

I have open sourced all of the code for this project, so I have no need to create a private container registry.

Going with a public registry also allows me to simplify the deployment process with Azure Container Apps as I don't need to set up any credentials in Azure Container Apps to get access to the container registry.

### Why use Terraform?

Terraform is a popular IAC (Infrastructure as Code) framework that works well with the popular cloud providers.

You can describe the cloud resources you require in a YAML formatted configuration file.

From there you can use the Azure or AWS CLI to login using your cloud credentials, then use Terraform to provision the resources you need using these on the accounts for the cloud platforms that you use.

### Provisioning a Public Container Repository on Amazon ECR with Terraform

I created three files to provision a public container repository on Amazon ECR with Terraform.

- `ecr.tf`: which describes the public Amazon ECR repository to provision with Terraform
- `variables.tf` and `dev.tfvars`: to provide configuration variables to Terraform to provision these resources such as the AWS region and the ECR repository name

See these files below.

{: file='ecr.tf'}

```terraform
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "=6.10.0"
    }
  }
}

provider "aws" {
    region = var.aws_region
}

resource "aws_ecrpublic_repository" "ecr_repo" {
  repository_name = var.aws_ecr_repo_name
}
```

{: file='variables.tf'}

```terraform
variable "aws_region" {
    type = string
    sensitive = true
}

variable "aws_ecr_repo_name" {
    type = string
    sensitive = true
}
```

{: file='dev.tfvars'}

```terraform
aws_region=<your_aws_region>
aws_ecr_repo_name=<your_ecr_repo_name>
```

You are going to want to keep tfvars files out of source control and your Docker container image to prevent sensitive data from leaking.

Before you can provision the cloud resource with Terraform, you will need to log into your AWS account using the AWS CLI with `aws configure`.

To provision the public Amazon Elastic Container repository with Terraform run the following commands.

```
terraform init
terraform apply -var-file="dev.tfvars"
```

If for whatever reason you need to delete these resources use the `terraform destroy` command.

```
terraform destroy -var-file="dev.tfvars"
```

### Pushing the Docker Image to ECR

Now that your public container repository has been created you can now push your container image into ECR.

Use the following commands to build your Docker container image and push it into ECR.

Note to replace the region "us-east-1" with the region you selected and the `<publicECRAlias>` with whatever your public ECR alias is.

There is a default alias for your account but you can also set a custom alias via the [AWS console](https://docs.aws.amazon.com/AmazonECR/latest/public/public-registry-settings.html).

```
aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/<publicECRAlias>
docker build -t streamlit .
docker tag streamlit:latest public.ecr.aws/<publicECRAlias>/streamlit:latest
docker push public.ecr.aws/<publicECRAlias>/streamlit:latest
```

I had a big issue preventing me from authenticating into my public repository using the `aws ecr-public get-login-password` command on a Windows machine, which is apparently a well known issue.

After a few attempts to solve the issue, I ended up falling back on GitHub Actions as a solution to enable me to push my container image into my public Amazon Elastic Container Registry which I will cover in a later post in this [series](/categories/streamlit-to-azure-container-apps-with-iac-and-cicd/).

## Related Posts

This post is part 4 of a [series](/categories/streamlit-to-azure-container-apps-with-iac-and-cicd/) of posts where I will write about my solution for getting a streamlit application deployed to Azure Container Apps with IAC and CICD.

See all of the posts included in this [series](/categories/streamlit-to-azure-container-apps-with-iac-and-cicd/) below.

{% assign category = "Streamlit to Azure Container Apps with IAC and CICD" %}

<ul>
  {% for post in site.categories[category] reversed %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
