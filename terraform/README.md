# Terraform Interview Questions


## 1. What is Terraform, Why should we use it?

Terraform is a popular IaC (Infrastructure as Code) tool. It is widely used due to its advantages over manual infrastructure creation.

### Key Benefits


- **Version Control:** Infrastructure as code can be versioned in Git, enabling history tracking and collaboration.
- **Consistent Infra:** Ensures consistent configurations across environments (DEV, QA, PROD, etc.).
- **Automated Infra CRUD:** Enables quick creation, updating, and deletion of infrastructure, reducing human errors.
- **Inventory Management:** Easily track resources across regions.
- **Cost Optimisation:** Quickly create or delete infrastructure as needed to save costs.
- **Automatic Dependency Management:** Terraform understands and manages resource dependencies.
- **Modular Infra:** Supports code reuse via modules.

---

## 2. Ansible vs Terraform: When to use? What is the purpose?

- **Ansible:** Configuration management tool, best for automating server configurations and tasks inside servers.
- **Terraform:** Infrastructure provisioning tool, best for creating, updating, and destroying infrastructure.

> **Integration:** Terraform can provision servers, then hand over configuration management to Ansible.

---

## 3. Explain Terraform Commands

- `terraform init`: Initializes the provider and downloads dependencies.
- `terraform plan`: Shows the execution plan without making changes.
- `terraform apply`: Applies the changes to create/update infrastructure.
- `terraform destroy`: Destroys the managed infrastructure.
- `terraform fmt`: Formats Terraform files.
- `terraform taint`: Marks a resource for recreation.
- `terraform import`: Imports existing resources into Terraform.
- `terraform -target`: Targets specific resources for apply.
- `terraform get -update`: Updates modules.
- `terraform workspace`: Manages workspaces.

---

## 4. What is the state in Terraform?

Terraform uses a state file (`terraform.tfstate`) to track the actual infrastructure and compare it with the desired state defined in `.tf` files.

---

## 5. What is the remote state in Terraform?

Storing state locally is not recommended. Use a centralized remote state (e.g., S3 with DynamoDB locking) for collaboration and to avoid conflicts.

---

## 6. Explain variables in Terraform

Variables parameterize infrastructure, allowing dynamic values in configurations.

**Types:**
- String
- Number
- Bool
- Map
- List

---

## 7. What is TFVARS in Terraform?

TFVARS files provide key-value pairs to supply variable values. They can override defaults and support multiple environments (e.g., `dev.tfvars`, `prod.tfvars`).

```sh
terraform apply -var-file=dev.tfvars
```

---

## 8. What is count and count.index in Terraform?

- `count`: Creates multiple instances of a resource.
- `count.index`: Refers to the index (starting from 0) for each instance.

---

## 9. What are outputs in Terraform?

Outputs expose resource attributes after creation, useful for chaining resources or modules.

---

## 10. What are data sources in Terraform?

Data sources query existing resources in the cloud, allowing their attributes to be used in new resources.

---

## 11. What are locals in Terraform?

Locals are key-value pairs for intermediate values or expressions. Unlike variables, locals cannot be overridden.

---

## 12. What are functions in Terraform?

Functions perform operations on inputs and return outputs. Common functions:

- `file`
- `split`
- `slice`
- `element`
- `length`
- `lookup`
- `join`

---

## 13. How to Store Sensitive Data in Terraform?

Use Vault, AWS Secrets Manager, or Parameter Store. Avoid hardcoding credentials; use environment variables or CI/CD secrets management.

---

## 14. How do you manage credentials in Terraform?

- Use environment variables.
- Store credentials in CI/CD tools (e.g., Jenkins with `withCredentials`).
- Assign IAM roles to EC2 instances running Terraform.

---

## 15. Explain the concept of a workspace

Workspaces allow managing multiple environments (e.g., dev, prod) with the same code. Use `terraform.workspace` to control environment-specific variables.

---

## 16. How does Terraform manage updates to existing resources?

Terraform compares the state file with the actual infrastructure and applies changes to match the desired state.

---

## 17. Example: Create a single EC2 instance on AWS

Practice creating an EC2 instance using separate files (`variables.tf`, `provider.tf`, `main.tf`, etc.) and modules for best practices.

---

## 18. How to Store Sensitive Data in Terraform?

Use Vault, AWS Secrets Manager, or Parameter Store for secure storage and retrieval of sensitive data.

---

## 19. Does Terraform support multi-provider deployments?

Yes, define multiple providers in your configuration to manage resources across different platforms.

---

## 20. If I lose the state file in Terraform, how can I recover it?

- Use remote state with versioning and replication.
- Restrict access to the state file.
- Enable MFA delete.
- Restore from backups if lost.

---

## 21. What is a null resource in Terraform?

A resource that doesn't create infrastructure but can use provisioners (e.g., `local-exec`, `remote-exec`, `file`).

---

## 22. What are modules in Terraform?

Modules enable code reuse, centralization, and consistency across projects and environments.

---

## 23. Is there a tool that can look for security vulnerabilities in your Terraform code?

Yes, tools like `tflint` (for linting and validation) and others can be integrated into CI/CD pipelines.

---

## 24. Difference between Ansible & Terraform

- **Terraform:** IaC tool for provisioning infrastructure.
- **Ansible:** Configuration management tool for server setup.
- **Integration:** Use Terraform for infra, Ansible for configuration.

---

## 25. Terraform vs CloudFormation

- **Terraform:** Multi-cloud, state management, extensible providers.
- **CloudFormation:** AWS-only, manages via stacks, less extensible.

---

## 26. What happens if you manually change resources created by Terraform?

Terraform will detect changes during refresh and attempt to revert manual changes to match the declared state.

---

## 27. How do you deploy your infra code in different environments?

- Use workspaces.
- Use different TFVARS files.
- Maintain separate codebases if needed, but reuse modules.

---

## 28. What are provisioners in Terraform?

Provisioners execute scripts or commands after resource creation.

- **local-exec:** Runs locally.
- **remote-exec:** Runs on the remote resource via SSH.
- **file:** Copies files to remote resources.

**Example:**

```hcl
resource "aws_instance" "example" {
    ami           = "ami-12345678"
    instance_type = "t2.micro"

    provisioner "local-exec" {
        command = "echo Instance Created: ${self.public_ip} >> instances.txt"
    }

    connection {
        type        = "ssh"
        user        = "ec2-user"
        private_key = file("~/.ssh/id_rsa")
        host        = self.public_ip
    }

    provisioner "remote-exec" {
        inline = [
            "sudo yum update -y",
            "sudo yum install -y nginx",
            "sudo systemctl start nginx"
        ]
    }

    provisioner "file" {
        source      = "app.conf"
        destination = "/etc/nginx/nginx.conf"
    }
}
```

---

## 29. Terraform apply fails due to a state file lock issue. How would you resolve it?

- Use `terraform force-unlock LOCK_ID` to release the lock.
- Manually delete the lock entry in DynamoDB if using remote state locking.

---

## 30. Some resources are not deleted when destroying infrastructure. What went wrong?

- Resource dependencies (e.g., security groups attached to EC2).
- Insufficient permissions to delete resources.
- Ensure dependencies are removed before deleting parent resources.

---
