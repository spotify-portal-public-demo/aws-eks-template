# ${{ values.component_id }}

${{ values.description }}

## Overview

This repository contains the Terraform configuration for the **${{ values.component_id }}** EKS cluster, provisioned via the AWS EKS Backstage template.

| Property | Value |
|---|---|
| Cluster name | `${{ values.component_id }}` |
| AWS region | `${{ values.region }}` |
| Owner | `${{ values.owner }}` |

## AWS resources

| Resource | Name |
|---|---|
| EKS cluster | `${{ values.component_id }}` |
| EKS node group | `${{ values.component_id }}-nodes` |
| Cluster IAM role | `${{ values.component_id }}-cluster-role` |
| Node IAM role | `${{ values.component_id }}-node-role` |

The cluster runs in the default VPC of the chosen region. Terraform state is stored in S3.

## Connecting to the cluster

```bash
aws eks update-kubeconfig --region ${{ values.region }} --name ${{ values.component_id }}
```

Then verify your nodes are ready:

```bash
kubectl get nodes
```

> The IAM principal you use must either be the one that created the cluster, or have been added to the cluster's `aws-auth` ConfigMap.

## Deploying changes

Changes to the Terraform configuration are applied via the **Terraform Apply** GitHub Actions workflow. Open a pull request — the plan workflow runs automatically on PRs, and apply runs on merge to `main`.
