# Terraform State Management & Backend

## Overview

Terraform uses a **State File** to map real-world infrastructure resources with Terraform configuration files (`.tf`). The state file stores metadata about managed resources and helps Terraform track infrastructure changes. By default, Terraform stores the state locally in a file named `terraform.tfstate`. 【1-80ea50】

## What is Terraform State?

Terraform State is a snapshot of your infrastructure that Terraform uses to:

- Track resources created and managed by Terraform.
- Compare the current state with the desired configuration.
- Store resource metadata and output values.
- Maintain infrastructure consistency. 

### State Files

| File | Description |
|--------|-------------|
| `terraform.tfstate` | Current state of infrastructure |
| `terraform.tfstate.backup` | Backup of previous state |

> **Note:** It is not recommended to edit state files manually. 

---

# Terraform State Commands

### View Outputs

```bash
terraform output
```

Displays values from output blocks stored in the state file. 

### Show State Content

```bash
terraform show
```

Displays the contents of a state file or plan file. 

### List Resources

```bash
terraform state list
```

Shows all resources tracked in the state file. 

### Show Resource Details

```bash
terraform state show <resource_name>
```

Displays detailed information about a specific resource. 

---

# Terraform Backend

A **Backend** defines where Terraform stores its state file. State files can be stored locally or remotely. Supported backend types include:

- Local
- AWS S3 (`s3`)
- Azure Storage (`azurerm`)
- Google Cloud Storage (`gcs`) 

---

# Why Use a Remote Backend?

Remote backends provide:

- Collaboration
- State Locking
- Encryption
- Versioning 

Benefits:

- Allows multiple team members to work together.
- Protects state files from unauthorized access.
- Prevents simultaneous updates.
- Maintains state history and recovery options.

---

# AWS Backend Example (S3)

## Prerequisites

- AWS Account
- S3 Bucket
- Terraform Installed

## Backend Configuration

```hcl
terraform {
  backend "s3" {
    bucket = "terraform-state-bucket"
    key    = "prod/terraform.tfstate"
    region = "us-east-1"
  }
}
```

## Recommended Production Configuration

```hcl
terraform {
  backend "s3" {
    bucket  = "terraform-state-bucket"
    key     = "prod/terraform.tfstate"
    region  = "us-east-1"
    encrypt = true
  }
}
```

## Initialize Backend

```bash
terraform init
```

### Advantages

- Centralized state storage
- Versioning support
- Team collaboration
- Secure storage with encryption

---

# Azure Backend Example (AzureRM)

## Prerequisites

- Azure Subscription
- Resource Group
- Storage Account
- Blob Container
- Terraform Installed

## Backend Configuration

```hcl
terraform {
  backend "azurerm" {
    resource_group_name  = "terraform-rg"
    storage_account_name = "terraformstorage"
    container_name       = "tfstate"
    key                  = "prod.terraform.tfstate"
  }
}
```

## Initialize Backend

```bash
terraform init
```

### Advantages

- Secure remote state storage
- Azure RBAC Integration
- Blob versioning capability
- Team collaboration

---

# Available Terraform Backends

| Backend | Platform |
|----------|----------|
| local | Local Machine |
| s3 | AWS |
| azurerm | Microsoft Azure |
| gcs | Google Cloud |
| consul | HashiCorp Consul |
| remote | Terraform Cloud |



---

# Terraform Import

Terraform Import allows existing cloud resources to be brought under Terraform management. 

## Import Workflow

1. Identify the existing resource.
2. Define its configuration in a Terraform file.
3. Run the import command.
4. Review the plan output.
5. Update the configuration if required. 

## AWS Example

```bash
terraform import aws_s3_bucket.mybucket existing-bucket-name
```

## Azure Example

```bash
terraform import azurerm_resource_group.rg my-resource-group
```

---

# Terraform Provisioners

Provisioners execute actions after infrastructure creation or destruction and are defined inside resource blocks. 【1-80ea50】

Types of Provisioners:

- file
- local-exec
- remote-exec 

### Example: local-exec

```hcl
resource "null_resource" "example" {
  provisioner "local-exec" {
    command = "echo Infrastructure Created"
  }
}
```

---

# Best Practices

- Store state remotely in production environments.
- Enable encryption for state storage.
- Enable versioning on backend storage.
- Use state locking mechanisms.
- Never manually edit state files.
- Regularly back up state data. 

---

# Learning Objectives

After completing this module, you will be able to:

- Understand Terraform State Management.
- Use Terraform State Commands.
- Configure Local and Remote Backends.
- Implement AWS and Azure Remote Backends.
- Import Existing Infrastructure into Terraform. 【

---
