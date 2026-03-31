# AWS EKS Template

This Backstage software template provisions a production-ready Amazon EKS (Elastic Kubernetes Service) cluster on AWS using Terraform.

## Overview

The template scaffolds a new GitHub repository containing all the Terraform configuration and GitHub Actions workflows needed to provision and manage an EKS cluster in your AWS account. Once created, the cluster is automatically registered in the Portal catalog for full visibility.

## What Gets Created

When you run this template, the following AWS resources are provisioned:

- **EKS Cluster** — a managed Kubernetes control plane in your chosen AWS region
- **Managed Node Group** — worker nodes that scale automatically to meet demand
- **IAM Roles** — dedicated roles for the EKS control plane and node group, following AWS least-privilege best practices
- **Terraform State** — stored in S3 for safe, collaborative infrastructure management

## Prerequisites

Before using this template ensure the following are in place:

- An **S3 bucket** for Terraform state storage
- **AWS credentials** available to GitHub Actions with sufficient permissions to create EKS, IAM, and EC2 resources
- A **GitHub integration** in your Backstage instance with permission to create repositories and GitHub Actions workflows

## Usage

1. Click **Create** on the template
2. Fill in the cluster name, owner, and AWS region
3. Specify the GitHub repository location
4. Click **Create** — Portal will scaffold the repo and GitHub Actions will apply the Terraform automatically

## Connecting to Your Cluster

Once the cluster is provisioned, configure `kubectl` by running:

```bash
aws eks update-kubeconfig --region <region> --name <cluster-name>
```

## Further Reading

- [Amazon EKS Documentation](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html)
- [AWS Terraform Provider](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [Backstage Software Templates](https://backstage.io/docs/features/software-templates/)
