 Terraform Introduction

A beginner-friendly Terraform learning module covering Infrastructure as Code concepts, Terraform architecture, core components, installation prerequisites, commands, workflow, and Terraform configuration files for AWS, google cloud and Microsoft Azure.


## Table of Contents

- [Overview](#overview)
- [Module Goals](#module-goals)
- [Why Terraform?](#why-terraform)
- [Core Components](#core-components)
- [Terraform Architecture](#terraform-architecture)
- [Prerequisites](#prerequisites)
- [Authentication](#authentication)
  - [AWS Authentication](#aws-authentication)
  - [Azure Authentication](#azure-authentication)
- [Terraform Workflow](#terraform-workflow)
- [Primary Terraform Commands](#primary-terraform-commands)
- [Terraform Configuration Files](#terraform-configuration-files)
  - [Provider](#provider)
  - [Resource](#resource)
  - [Data Source](#data-source)
- [Terraform with Azure](#terraform-with-azure)
  - [AzureRM Provider](#azurerm-provider)
  - [Azure Resource Group Example](#azure-resource-group-example)
  - [Azure Virtual Network Example](#azure-virtual-network-example)
  - [Remote State in Azure Storage](#remote-state-in-azure-storage)
  - [Azure Virtual Desktop Notes](#azure-virtual-desktop-notes)
- [Recommended Repository Structure](#recommended-repository-structure)
- [Getting Started](#getting-started)
  - [AWS Example Flow](#aws-example-flow)
  - [Azure Example Flow](#azure-example-flow)
- [Best Practices](#best-practices)
- [References](#references)
- [License](#license)

## Overview

Terraform is an Infrastructure as Code tool used to provision and manage infrastructure across cloud providers. It is open source, uses HCL or JSON syntax, supports immutable infrastructure, and is platform agnostic.

Terraform helps teams define infrastructure declaratively, review planned changes before applying them, and manage infrastructure consistently through reusable configuration files.

Terraform can be used with multiple cloud platforms, including:

- Amazon Web Services
- Microsoft Azure
- Google Cloud
- VMware
- OpenShift

## Module Goals

By the end of this module, learners should be able to:

- Explain Terraform and its key features
- Understand Terraform architecture
- Develop Terraform scripts to create infrastructure
- Describe the working of Terraform commands
- Work with AWS Cloud using Terraform
- Work with Microsoft Azure using Terraform
- Understand AzureRM provider basics
- Store Terraform state securely in Azure Storage

## Why Terraform?

Terraform is useful when infrastructure needs to be automated, repeatable, and manageable across environments or cloud platforms.

Key characteristics include:

- Open source and free, with enterprise support available
- Push-based provisioning model
- Declarative configuration style
- Idempotent execution
- Consistent infrastructure provisioning
- Multi-cloud and hybrid-cloud support
- Integration with DevOps tools
- Dry-run support through execution plans
- Versioning-friendly configuration files
- Scalable infrastructure management

## Core Components

Terraform includes the following core components:

- **Executable**: The Terraform CLI used to run commands
- **Configuration Files**: Manifest files such as `.tf` and `.tfvars`
- **Provider Plugins**: Plugins that allow Terraform to interact with cloud providers
- **State File**: A file such as `terraform.tfstate` that tracks managed infrastructure

## Terraform Architecture

At a high level, Terraform works as follows:

1. A developer writes Terraform manifest files such as `.tf` and `.tfvars`.
2. Terraform Core reads the configuration.
3. Provider plugins connect Terraform to cloud service providers.
4. Terraform creates, modifies, or deletes infrastructure based on the desired state.
5. Terraform records infrastructure state in the state file.

## Terraform Architecture

At a high level, Terraform works as follows:

1. A developer writes Terraform manifest files such as `.tf` and `.tfvars`.
2. Terraform Core reads the configuration.
3. Provider plugins connect Terraform to cloud service providers.
4. Terraform creates, modifies, or deletes infrastructure based on the desired state.
5. Terraform records infrastructure state in the state file.

## Prerequisites

Before using Terraform, install the tools required for your target cloud platform.

### Common Tools

- Terraform binary
- Git
- Code editor such as Visual Studio Code

### AWS Tools

- AWS CLI
- AWS account credentials or role-based access

### Azure Tools

- Azure CLI
- Azure subscription
- Microsoft Entra ID access
- Required Azure role assignments for the resources you plan to manage
- Azure Storage account and Blob container if using remote backend state

## Authentication

Terraform requires authentication to communicate with the target cloud provider APIs.

### AWS Authentication

Configure AWS CLI authentication before running Terraform against AWS resources:

```bash
aws configure
```

You will typically be prompted for:

- AWS Access Key ID
- AWS Secret Access Key
- Default region name
- Default output format

### Azure Authentication

For local Azure development, sign in using Azure CLI:

```bash
az login
```

Check the active subscription:

```bash
az account show
```

Set the required subscription if needed:

```bash
az account set --subscription "<subscription-id>"
```

For automation or CI/CD pipelines, prefer identity-based authentication such as:

- Service principal
- Managed identity
- OpenID Connect or workload identity federation

> **Security note:** Do not commit credentials, access keys, secret keys, client secrets, certificates, `.tfvars` files containing sensitive values, or local state files to GitHub.

## Terraform Workflow

A typical Terraform workflow follows this order:

```bash
terraform init
terraform validate
terraform plan
terraform apply
terraform destroy
```

## Primary Terraform Commands

### `terraform init`

Initializes the working directory.

It is used to:

- Initialize the backend
- Download modules
- Download providers
- Create a dependency lock file

```bash
terraform init
```

### `terraform validate`

Validates the Terraform configuration files.

It checks:

- Configuration correctness
- Syntax validity

```bash
terraform validate
```

### `terraform plan`

Creates an execution plan before making infrastructure changes.

It is commonly used as a dry run to preview what Terraform will create, update, or destroy.

```bash
terraform plan
```

You can also save a plan to a file:

```bash
terraform plan -out=tfplan
```

### `terraform apply`

Creates or modifies infrastructure in the cloud according to the execution plan.

```bash
terraform apply
```

If you saved a plan file:

```bash
terraform apply tfplan
```

### `terraform destroy`

Deletes infrastructure managed by Terraform.

```bash
terraform destroy
```

> **Warning:** `terraform destroy` removes managed infrastructure. Review the plan carefully before confirming.

## Terraform Configuration Files

Terraform configurations are usually written in files with the `.tf` extension.

Common Terraform blocks include providers, resources, data sources, variables, outputs, and backend configuration.

### Provider

A provider tells Terraform which cloud or platform to interact with.

AWS example:

```hcl
provider "aws" {
  region = "us-east-1"
}
```

Azure example:

```hcl
provider "azurerm" {
  features {}
}
```

### Resource

A resource defines the infrastructure component Terraform should create or manage.

AWS S3 bucket example:

```hcl
resource "aws_s3_bucket" "example" {
  bucket = "my-example-bucket"
}
```

Azure resource group example:

```hcl
resource "azurerm_resource_group" "example" {
  name     = "rg-terraform-demo"
  location = "Central India"
}
```

Reference format:

```text
<resource_type>.<name>.<argument>
```

### Data Source

A data source performs read-only lookups against existing infrastructure.

AWS AMI example:

```hcl
data "aws_ami" "example" {
  most_recent = true
}
```

Azure resource group data source example:

```hcl
data "azurerm_resource_group" "existing" {
  name = "existing-resource-group"
}
```

Reference format:

```text
data.<resource_type>.<name>.<argument>
```

## Terraform with Azure

Terraform uses the AzureRM provider to create and manage Azure resources through Azure Resource Manager APIs.

Common Azure resources managed with Terraform include:

- Resource groups
- Virtual networks and subnets
- Network security groups
- Virtual machines
- Storage accounts
- Azure Kubernetes Service
- Key Vault
- Azure Virtual Desktop resources

### AzureRM Provider

Use the `required_providers` block to define the AzureRM provider source and version.

```hcl
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 5.0"
    }
  }
}

provider "azurerm" {
  features {}
}
```

If your environment requires explicit subscription configuration, use variables instead of hardcoded values:

```hcl
provider "azurerm" {
  features {}
  subscription_id = var.azure_subscription_id
  tenant_id       = var.azure_tenant_id
}
```

Example variables:

```hcl
variable "azure_subscription_id" {
  description = "Azure subscription ID"
  type        = string
  sensitive   = true
}

variable "azure_tenant_id" {
  description = "Microsoft Entra tenant ID"
  type        = string
  sensitive   = true
}
```

### Azure Resource Group Example

```hcl
resource "azurerm_resource_group" "main" {
  name     = "rg-terraform-demo"
  location = "Central India"

  tags = {
    environment = "dev"
    iac         = "terraform"
  }
}
```

### Azure Virtual Network Example

```hcl
resource "azurerm_virtual_network" "main" {
  name                = "vnet-terraform-demo"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  address_space       = ["10.0.0.0/16"]
}

resource "azurerm_subnet" "default" {
  name                 = "snet-default"
  resource_group_name  = azurerm_resource_group.main.name
  virtual_network_name = azurerm_virtual_network.main.name
  address_prefixes     = ["10.0.1.0/24"]
}
```

### Remote State in Azure Storage

For team-based projects, store Terraform state remotely instead of keeping only a local `terraform.tfstate` file.

Azure remote state commonly uses the `azurerm` backend with Azure Blob Storage.

```hcl
terraform {
  backend "azurerm" {
    resource_group_name  = "rg-tfstate"
    storage_account_name = "sttfstateexample"
    container_name       = "tfstate"
    key                  = "dev.terraform.tfstate"
  }
}
```

Recommended approach:

- Use Microsoft Entra ID authentication where possible
- Restrict access to the state container
- Do not hardcode secrets in backend configuration
- Use separate state files for separate environments
- Enable storage account security controls based on your organization standards

### Azure Virtual Desktop Notes

Terraform can be used to manage Azure Virtual Desktop infrastructure components such as:

- Host pools
- Application groups
- Workspaces
- Session host virtual machines
- Network interfaces
- VM extensions

Example AVD-style resource names:

```hcl
resource "azurerm_virtual_desktop_host_pool" "hp" {
  name                = "avd-hp-prod"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  type                = "Pooled"
  load_balancer_type  = "BreadthFirst"
}
```

> For production AVD deployments, validate naming strategy, session host scaling, extensions, image management, identity join type, Intune enrollment requirements, and Terraform state behavior in a non-production environment before rollout.

## Recommended Repository Structure

```text
terraform-introduction/
├── README.md
├── main.tf
├── providers.tf
├── backend.tf
├── variables.tf
├── outputs.tf
├── terraform.tfvars.example
├── .gitignore
└── modules/
```

Recommended `.gitignore` entries:

```gitignore
.terraform/
*.tfstate
*.tfstate.*
*.tfvars
.terraform.lock.hcl
crash.log
crash.*.log
override.tf
override.tf.json
*_override.tf
*_override.tf.json
```

> Commit `terraform.tfvars.example` for sample values, but do not commit real `terraform.tfvars` files if they contain environment-specific or sensitive values.

## Getting Started

Clone the repository:

```bash
git clone <repository-url>
cd terraform-introduction
```

### AWS Example Flow

Initialize Terraform:

```bash
terraform init
```

Validate configuration:

```bash
terraform validate
```

Preview changes:

```bash
terraform plan
```

Apply changes:

```bash
terraform apply
```

Destroy resources when no longer needed:

```bash
terraform destroy
```

### Azure Example Flow

Sign in to Azure:

```bash
az login
```

Set subscription if required:

```bash
az account set --subscription "<subscription-id>"
```

Initialize Terraform:

```bash
terraform init
```

Validate configuration:

```bash
terraform validate
```

Preview Azure changes:

```bash
terraform plan
```

Deploy Azure resources:

```bash
terraform apply
```

Remove Azure resources when no longer required:

```bash
terraform destroy
```

## Best Practices

- Keep Terraform files version controlled.
- Use remote state for team-based projects.
- Do not commit secrets or credentials.
- Use variables for reusable configuration.
- Run `terraform validate` before `terraform plan`.
- Review `terraform plan` carefully before applying changes.
- Use modules to organize reusable infrastructure code.
- Use clear naming conventions for resources and variables.
- Separate environments using workspaces, folders, or separate state files.
- Use Azure role-based access control with least privilege.
- Prefer identity-based authentication over static secrets for automation.
- Test infrastructure changes in non-production before production deployment.

## References

- [Terraform AzureRM Provider Documentation](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs)
- [Terraform on Azure Documentation](https://learn.microsoft.com/en-us/azure/developer/terraform/)
- [Terraform AzureRM Backend Documentation](https://developer.hashicorp.com/terraform/language/backend/azurerm)

## License

Add your project license here.

Example:

```text
MIT License
```
