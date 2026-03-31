# aws-eks-template

Example Backstage template for creating an EKS cluster in AWS using Terraform.

## Overview

This template demonstrates how to use [Spotify Portal](https://backstage.spotify.com) Software Templates
to give developers self-service access to AWS infrastructure. When run, it scaffolds a new GitHub
repository containing Terraform configuration and GitHub Actions workflows that automatically
provision a production-ready EKS cluster in your AWS account.

The template uses the [AWS Terraform provider](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
and creates the following resources:

- **IAM roles and policies** — dedicated roles for the EKS control plane and managed node group, following AWS least-privilege best practices
- **EKS cluster** — a Kubernetes cluster provisioned into your existing VPC, spread across multiple availability zones for resilience
- **Managed node group** — worker nodes that can scale to meet demand, managed by AWS
- **Terraform state backend** — an S3 bucket is used to store and manage Terraform state, enabling safe collaboration and future changes through standard pull request workflows

## Prerequisites

Before using this template you will need:

### 1. Terraform State Bucket

An S3 bucket is required to store Terraform state. This allows Terraform to track what
infrastructure exists and safely manage changes over time. Create a bucket in your AWS account
and update the `bucket` value in `skeleton/main.tf` to match.

### 2. AWS Credentials

When the GitHub Actions workflow runs, Terraform needs permission to create and manage
resources in your AWS account. How you provide these credentials will depend on your
organisation's security policies and existing AWS setup. Common approaches include:

- **GitHub organisation secrets** — store `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` as org-level secrets so any repository created by this template automatically inherits them
- **IAM roles with OIDC** — the recommended approach for production, using GitHub's OIDC provider to assume an IAM role without storing long-lived credentials
- **Environment-specific secrets** — scoping credentials to specific GitHub environments for additional control
- **Third-party secret managers** — such as HashiCorp Vault or AWS Secrets Manager, integrated into your GitHub Actions workflow

Whichever approach you use, the credentials will need sufficient permissions to create and manage EKS clusters, IAM roles, VPC resources, and read/write to the Terraform state bucket.

### 3. GitHub Integration

The GitHub integration in your Backstage instance (Personal Access Token or GitHub App)
will need permission to read and write GitHub Actions. This allows the template to create
repositories with working CI/CD workflows out of the box.

## How It Works

1. A developer fills in a simple form in Portal — cluster name, owner, and AWS region
2. Portal fetches this template, injects the provided values, and creates a new GitHub repository
3. GitHub Actions automatically triggers and runs `terraform apply`
4. All AWS resources are provisioned without the developer needing AWS console access
5. The cluster is registered in the Portal catalog for full visibility

## Notes

This template is intended as an example and starting point. The approach to credentials
and permissions described here should be reviewed and adapted to meet your organisation's
security requirements before use in production.
