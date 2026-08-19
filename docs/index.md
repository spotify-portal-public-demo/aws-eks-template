# AWS EKS Template

This Backstage software template provisions a production-ready Amazon EKS (Elastic Kubernetes Service) cluster on AWS using Terraform. It creates a new GitHub repository pre-populated with Terraform configuration, GitHub Actions workflows for plan and apply, and a Backstage catalog entry — all from a single form in the Spotify Portal.

## What this template does

When you run this template it will:

1. Scaffold a Terraform project from the built-in skeleton.
2. Create a new GitHub repository under the configured GitHub organisation.
3. Push the scaffolded code to that repository with a `main` default branch.
4. Register the new cluster as a `Resource` entity in the Backstage catalog.

## AWS resources created

The Terraform configuration provisions the following resources:

| Resource | Description |
|---|---|
| `aws_eks_cluster` | The EKS control plane, named after the cluster ID you provide. |
| `aws_eks_node_group` | A managed node group with a desired size of 2 nodes (min 1, max 3). |
| `aws_iam_role` (cluster) | IAM role for the EKS control plane, with `AmazonEKSClusterPolicy` attached. |
| `aws_iam_role` (nodes) | IAM role for worker nodes, with `AmazonEKSWorkerNodePolicy`, `AmazonEKS_CNI_Policy`, and `AmazonEC2ContainerRegistryReadOnly` attached. |
| `aws_iam_role_policy_attachment` (×4) | Policy attachments wiring the above roles to the required AWS managed policies. |

The cluster is deployed into the **default VPC** and its subnets in the chosen region. Terraform state is stored remotely in an S3 bucket.

## Prerequisites

Before using this template ensure you have the following in place:

- **AWS credentials** configured in your GitHub repository as Actions secrets. The workflows expect an IAM principal with permissions to manage EKS, IAM roles, and EC2 networking.
- **AWS CLI** installed locally (v2 recommended) if you want to connect to the cluster after provisioning.
- **`kubectl`** installed locally to interact with the cluster.
- **Permissions** in the Backstage catalog to create a `Resource` entity owned by the group you select.
- A **GitHub organisation** that matches the allowed owner configured in the template (`spotify-portal-public-demo` by default — update `template.yaml` to change this).

## How to use this template

1. Navigate to **Create** in the Spotify Portal and search for **AWS - EKS (Kubernetes)**.
2. Fill in the form:
   - **Name** — a unique identifier for the cluster (e.g. `my-team-eks`). This becomes the EKS cluster name, IAM role names, and node group name.
   - **Description** *(optional)* — a short description shown in the catalog.
   - **Owner** — the Backstage group that owns this cluster.
   - **AWS Region** — choose `us-east-1`, `eu-west-1`, or `ap-southeast-1`.
   - **Repository location** — the GitHub repository that will be created.
3. Click **Review**, then **Create**.
4. Wait for the scaffolding steps to complete. Once finished, links to the new repository and catalog entry are shown.
5. In the new repository, trigger the **Terraform Apply** GitHub Actions workflow to provision the cluster. It will take a few minutes for all resources to become available.

## Connecting to the cluster

Once the Terraform apply has completed successfully, update your local kubeconfig with:

```bash
aws eks update-kubeconfig --region <region> --name <cluster-name>
```

Replace `<region>` with the AWS region you chose (e.g. `eu-west-1`) and `<cluster-name>` with the name you gave the cluster.

Verify connectivity:

```bash
kubectl get nodes
```

You should see the worker nodes in a `Ready` state within a few minutes of the node group becoming active.

> **Note:** The IAM principal you use for `aws eks update-kubeconfig` must be the same one that created the cluster, or must be added to the cluster's `aws-auth` ConfigMap with appropriate permissions.
