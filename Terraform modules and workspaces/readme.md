# Terraform Modules & Workspaces

> Learn how to build reusable, scalable, and environment-aware Infrastructure as Code (IaC) using Terraform Modules and Workspaces.

---

## 📖 Overview

Terraform is an Infrastructure as Code (IaC) tool developed by HashiCorp that enables infrastructure provisioning through code rather than manual configuration.

This repository demonstrates:

- Terraform Workspaces
- Terraform Modules
- Environment Management
- Azure Infrastructure Examples
- AWS Infrastructure Examples
- Module Reusability Best Practices

---

## 🎯 Learning Objectives

After completing this module, you will be able to:

- Understand Terraform Workspaces
- Manage multiple environments using the same codebase
- Create reusable Terraform Modules
- Understand Root and Child Modules
- Use Terraform Registry Modules
- Implement industry-standard Terraform project structures

---

# What is Terraform?

Terraform allows teams to define infrastructure using declarative configuration files.

Common resources managed by Terraform include:

- Virtual Machines
- Virtual Networks
- Storage Accounts
- Databases
- Security Groups
- Kubernetes Clusters
- Cloud Networking Components

Benefits:

✅ Infrastructure as Code

✅ Automation

✅ Version Control

✅ Reusability

✅ Standardization

✅ Multi-Cloud Support

---

# Terraform Workspaces

## Definition

Terraform Workspaces allow multiple deployments of the same Terraform configuration while maintaining separate state files.

When Terraform is initialized, a workspace named:

```bash
default
```

is automatically created.

---

## Why Use Workspaces?

Workspaces are commonly used when deploying identical infrastructure across multiple environments.

Examples:

```text
Development
Testing
Staging
Production
```

Each workspace maintains its own state information while sharing the same Terraform code.

---

## Workspace Benefits

### Environment Isolation

```text
DEV
TEST
PROD
```

### Multiple Regions

```text
East US
West Europe
Central India
```

### Multiple Accounts

```text
AWS Dev Account
AWS Production Account

Azure Dev Subscription
Azure Production Subscription
```

### State Separation

Each workspace maintains a dedicated Terraform state file.

---

## Using terraform.workspace

The active workspace can be referenced using:

```hcl
terraform.workspace
```

Example:

```hcl
resource "azurerm_resource_group" "rg" {

  name     = "app-${terraform.workspace}-rg"

  location = "Central India"

}
```

Generated Resources:

```text
app-dev-rg
app-test-rg
app-prod-rg
```

---

# Workspace Commands

## List Workspaces

```bash
terraform workspace list
```

---

## Show Current Workspace

```bash
terraform workspace show
```

---

## Create Workspace

```bash
terraform workspace new dev
```

---

## Select Workspace

```bash
terraform workspace select dev
```

---

## Delete Workspace

```bash
terraform workspace delete dev
```

> Always destroy resources before deleting a workspace.

---

# Terraform Modules

## Definition

A Terraform Module is a collection of Terraform configuration files stored within a directory.

Modules are used to create reusable infrastructure components.

A module can contain:

```text
main.tf
variables.tf
outputs.tf
```

---

## Benefits of Modules

- Reusability
- Standardization
- Version Control
- Better Infrastructure Management
- Reduced Duplication
- Easier Maintenance

---

# Types of Terraform Modules

## Root Module

The Terraform configuration located in the current working directory.

Example:

```text
main.tf
variables.tf
outputs.tf
```

---

## Child Module

A reusable module invoked by another Terraform configuration.

Example:

```hcl
module "network" {

  source = "./modules/network"

}
```

---

## Published Module

Reusable modules published to:

- Terraform Registry
- GitHub
- Bitbucket
- Private Git Repositories

---

# Terraform Registry

Terraform Registry contains:

- Providers
- Official Modules
- Community Modules

Registry URL:

https://registry.terraform.io

Example:

```hcl
module "vpc" {

  source = "terraform-aws-modules/vpc/aws"

  version = "5.0.0"

}
```

---

# Module Source Types

Terraform supports multiple module sources.

### Local Path

```hcl
source = "./modules/network"
```

### Terraform Registry

```hcl
source = "terraform-aws-modules/vpc/aws"
```

### GitHub

```hcl
source = "github.com/company/network-module"
```

### Other Supported Sources

- Bitbucket
- Generic Git Repositories
- HTTP URLs
- Amazon S3 Buckets
- Google Cloud Storage Buckets

---

# Recommended Project Structure

```text
terraform-project/

├── main.tf
├── variables.tf
├── outputs.tf
├── provider.tf
├── terraform.tfvars

└── modules/

    ├── network/
    │   ├── main.tf
    │   ├── variables.tf
    │   └── outputs.tf

    ├── compute/
    │   ├── main.tf
    │   ├── variables.tf
    │   └── outputs.tf

    └── storage/
        ├── main.tf
        ├── variables.tf
        └── outputs.tf
```

---

# Azure Industry Example

## Scenario

A banking organization maintains three environments:

```text
DEV
UAT
PROD
```

Using Workspaces:

```bash
terraform workspace new dev
terraform workspace new uat
terraform workspace new prod
```

Resource Group Deployment:

```hcl
resource "azurerm_resource_group" "rg" {

  name     = "bank-${terraform.workspace}-rg"

  location = "Central India"

}
```

Result:

```text
bank-dev-rg
bank-uat-rg
bank-prod-rg
```

### Azure VNet Module

```hcl
module "vnet" {

  source = "./modules/network"

  vnet_name = "bank-${terraform.workspace}-vnet"

}
```

Benefits:

- Environment consistency
- Network standardization
- Reduced manual effort

---

# AWS Industry Example

## Scenario

An e-commerce organization deploys workloads across:

```text
Development
Staging
Production
```

Using the same Terraform codebase.

### EC2 Deployment

```hcl
resource "aws_instance" "web" {

  ami           = "ami-example"

  instance_type = "t2.micro"

  tags = {
    Environment = terraform.workspace
  }

}
```

Generated:

```text
web-dev
web-stage
web-prod
```

### AWS VPC Module

```hcl
module "vpc" {

  source  = "terraform-aws-modules/vpc/aws"

  version = "5.0.0"

  name = "ecommerce-vpc"

}
```

Benefits:

- Reusable architecture
- Standard subnet design
- Consistent deployments

---

# Best Practices

## Workspaces

✅ Use separate workspaces for environments

✅ Protect production environments

✅ Store state files remotely

✅ Follow naming standards

---

## Modules

✅ Design small reusable modules

✅ Version shared modules

✅ Use variables and outputs

✅ Avoid hardcoding values

✅ Publish reusable modules

---

# Key Takeaways

- Terraform Workspaces provide state isolation.
- Terraform Modules enable infrastructure reuse.
- Registry simplifies module consumption.
- Workspaces and Modules together improve scalability.
- Azure and AWS deployments benefit from standardized Terraform practices.

---

## References

- Terraform Registry: https://registry.terraform.io
- Module-5_Terraform_Modules_and_Workspace Training Material

---

## License

This repository is intended for learning, demonstration, and internal training purposes.
`
