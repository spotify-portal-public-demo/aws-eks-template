# aws-eks-template

Example Backstage template for creating an EKS cluster in AWS using Terraform.

## Usage

This template will create an EKS cluster in your AWS account
by using the [AWS Terraform provider](https://registry.terraform.io/providers/hashicorp/aws/latest/docs).
The Terraform backend is set to an S3 bucket called `spotify-portal-public-demo-terraform-aws`.

When run, the template creates a new GitHub repo where the Terraform configuration files are
added along with GitHub Action workflow files. These workflows are responsible for setting up
and running Terraform and ultimately creating the EKS cluster.

The Terraform creates the following AWS resources:
- IAM role and policies for the EKS control plane
- IAM role and policies for the EKS managed node group
- EKS cluster (using the default VPC and subnets)
- EKS managed node group (2 nodes, t3.medium)

## Permissions

The sections below explain how the permissions work for this Backstage template.
Please note however that this is not a recommended approach to setting up permissions
for Terraform with Backstage and should only serve as an example.

### AWS Credentials

When running the GitHub Action workflow, Terraform needs permission to create and
manage resources within your AWS account. This is accomplished by creating an
IAM user (or role) with the correct permissions and saving the credentials as
GitHub Secrets (`AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`) on the
`spotify-portal-public-demo` org. By using organisation secrets, any new repos
created in that org will automatically be set up with the proper AWS permissions.

The IAM user/role will need at minimum the following permissions:
- `AmazonEKSClusterPolicy`
- `AmazonEKSWorkerNodePolicy`
- IAM permissions to create roles and attach policies
- EC2 permissions to describe VPCs and subnets
- S3 permissions to read/write the Terraform state bucket

### GitHub Integration

When running a template which includes GitHub actions (like this one), the GitHub Backstage
integration, whether using a Personal Access Token or a GitHub App, will require permission
to read and write GitHub Actions. The PAT or GitHub App can be modified if this permission
does not already exist.
