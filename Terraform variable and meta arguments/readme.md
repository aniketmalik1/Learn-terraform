# Terraform Variables & Meta-Arguments

## Overview

Terraform Variables and Meta-Arguments help make Infrastructure as Code (IaC) reusable, scalable, and easier to manage.

This example uses Azure Virtual Desktop (AVD) resources to demonstrate the concepts.

---

# 1. Input Variable

### Definition
Input Variables are used to pass values into Terraform configurations, making the code reusable.

### Example

```terraform
variable "location" {
  description = "Azure Region"
  type        = string
  default     = "Central India"
}
```

Usage:

```terraform
resource_group_location = var.location
```

---

# 2. Output Variable

### Definition
Output Variables display important information after Terraform deployment.

### Example

```terraform
output "hostpool_name" {
  value = azurerm_virtual_desktop_host_pool.avd.name
}
```

Result:

```bash
hostpool_name = avd-hostpool
```

---

# 3. Local Variable

### Definition
Local Variables are used to store values that are reused multiple times within the configuration.

### Example

```terraform
locals {
  prefix = "avd-demo"
}
```

Usage:

```terraform
name = "${local.prefix}-rg"
```

Result:

```text
avd-demo-rg
```

---

# 4. Meta-Argument: count

### Definition
The `count` meta-argument creates multiple copies of a resource.

### Example

```terraform
resource "azurerm_windows_virtual_machine" "sessionhost" {
  count = 2

  name = "avd-vm-${count.index}"
}
```

Resources Created:

```text
avd-vm-0
avd-vm-1
```

Use Case:
Create multiple AVD Session Host VMs.

---

# 5. Meta-Argument: for_each

### Definition
The `for_each` meta-argument creates resources for each item in a map or set.

### Example

```terraform
resource "azurerm_network_security_rule" "rules" {
  for_each = {
    RDP   = 3389
    HTTPS = 443
  }

  name = each.key
}
```

Resources Created:

```text
RDP
HTTPS
```

Use Case:
Create multiple NSG rules for AVD.

---

# 6. Meta-Argument: depends_on

### Definition
The `depends_on` meta-argument creates an explicit dependency between resources.

### Example

```terraform
resource "azurerm_virtual_desktop_application_group" "appgroup" {

  depends_on = [
    azurerm_virtual_desktop_host_pool.hostpool
  ]
}
```

Use Case:
Ensure the Host Pool is created before the Application Group.

---

# Azure Virtual Desktop Example Architecture

Terraform deploys:

- Resource Group
- Virtual Network
- Host Pool
- Application Group
- Workspace
- Session Hosts

---

# Summary

| Concept | Purpose |
|----------|----------|
| Variable | Store user input values |
| Output | Display deployment results |
| Local | Reuse values internally |
| count | Create multiple resources |
| for_each | Create resources from a list or map |
| depends_on | Control resource creation order |

---

## Learning Outcome

After completing this module, you will be able to:

- Create reusable Terraform code
- Use Variables, Outputs, and Locals
- Deploy multiple Azure resources using count
- Create dynamic resources using for_each
- Manage dependencies using depends_on
- Deploy Azure Virtual Desktop infrastructure efficiently
