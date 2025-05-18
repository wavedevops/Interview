# Terraform Interview Questions


## What is Terraform, Why should we use it?

Terraform is a popular IaC (Infrastructure as a Code) tool. It is the best in the market now. It has multiple advantages over manually created infra.

- **Version Control:**  
Since it is code, we can maintain in Git to version control. We can completely maintain the history of infra and collaboration is easy.

- **Consistent Infra:**  
Often we face the problem of different configurations in different environments like DEV, QA, PROD, etc. Using terraform we can create similar infra in multiple environments with more reliability.

- **Automated Infra CRUD:**  
Using terraform we can create entire infra in minutes reducing the human errors. Updating infra using terraform is also easy. Using Terraform we can delete infra.

- **Inventory Management:**  
If we create infra manually it is very tough to maintain the inventory of resources in diff regions. But by seeing terraform you can easily tell the resources you are using in different regions.

- **Cost Optimisation:**  
When you need infra you can create in minutes. When you don't you can delete in minutes, so you can save the cost.

- **Automatic dependency management:**  
terraform can understand the dependency of resources. It can tell us the dependency clearly.

- **Modular Infra:**  
Code reuse. We can develop our own modules our use open source modules to reuse the infra code. instead of spending more time to create infra from the scratch we can reuse modules.

---

## Ansible vs Terraform When to use? What is the purpose?

Ansible is a popular configuration management tool. We can automate the configuration inside servers through Ansible. We can use Ansible to create configurations in external systems too like github repo creation, ec2 and route53 records. But when it comes to infrastructure, Ansible is very poor in updating and destroying infra. It may create duplicate resources and be very hard to write and destroy playbooks. State management is not available in ansible.

So finally ansible is the perfect choice to manage configurations inside servers, terraform is the perfect choice to create infrastructure. We can integrate terraform with ansible. Once terraform creates servers, it can handover configuration management to ansible.

---

## Explain terraform commands?

- `terraform init` - Initialization of the provider.
- `terraform plan` - Gives the plan of what to create, change, or destroy.
- `terraform apply` - Applies the plan and creates infra.
- `terraform destroy` - Destroys the infra.
- `terraform fmt` - Formats .tf files.
- `terraform taint` - Marks a resource to be recreated.
- `terraform import` - Imports existing resource to terraform.
- `terraform -target` - Targets specific resources.
- `terraform get -update` - Updates modules.
- `terraform workspace` - Manages workspaces.

---

## What is the state in Terraform?

Terraform uses a state file to compare actual infra with declared infra in `.tf` files.  
**State file = Actual Infrastructure created by terraform**  
**Desired Infrastructure = Declared through .tf files**

Terraform responsibility is to match desired infrastructure with actual infrastructure.

---

## What is the remote state in Terraform?

Saving state locally is not recommended in terraform. Because in a collaborative environment multiple persons would be working on the same infra, so there is a high possibility of duplications and errors if you save the state locally.

Terraform recommends storing the state in a centralised location, accessible by the entire team and it should be locked so that more people can’t do the changes in infrastructure at a time. Below is the sample code to store the state in S3 and lock it with Dynamo DB.

---

## Explain variables in terraform?

Variables are used to parameterize the infrastructure. We can provide dynamic values to the configuration through variables.

Refer `var.[variable-name]` wherever you want.

Types of variables:

- String
- Number
- Bool
- Map
- List

---

## What is TFVARS in terraform?

TFVARS is the file which has key, value pairs. It is used to supply the values to terraform variables. If a default value is specified in a variables file TFVARS can override those values.

We can use different TFVARS files like `dev.tfvars` and `prod.tfvars` to provision the infrastructure in multiple environments.

Command to apply:
```bash
terraform apply -var-file=dev.tfvars
```

---

## What is count and count index in terraform?

`count` is used to create multiple instances of a resource.  
`count.index` refers to the index from 0, useful for assigning dynamic values.

---

## What are outputs in Terraform?

Outputs return values from terraform resources.  
They are useful to pass values between modules and reuse created resource attributes.

---

## What are data sources in terraform?

Data sources allow you to fetch data from existing resources.  
They help to query cloud provider data and reuse it.

---

## What are Locals in Terraform?

Locals are used to define reusable values with expressions and functions.  
Unlike variables, locals can't be overridden via tfvars or CLI.

---

## What are functions in terraform and name a few functions you used?

Functions help in processing data. Common functions include:

- `file()`
- `split()`
- `slice()`
- `element()`
- `length()`
- `lookup()`
- `join()`

---

## How to Store Sensitive Data in Terraform?

Use:
- Vault
- AWS Secrets Manager
- Secure Parameters

Use environment variables for credentials or assign IAM roles to EC2 running terraform.

---

## Explain the concept of a workspace?

Terraform workspace helps manage multiple environments using the same code.  
Use `terraform.workspace` to manage per-environment logic.

---

## How does Terraform manage updates to existing resources?

Terraform compares actual infra (state file) with desired infra.  
On `plan` or `apply`, it detects drift and suggests corrections.

---

## Give the terraform configuration for creating a single EC2 instance on AWS?

You must practise at least an example to create an ec2 instance by sharing your screen.  
Use `variables.tf`, `provider.tf`, `main.tf`, `data.tf`, `local.tf`, etc.

---

## How to Store Sensitive Data in Terraform?

Use vault, AWS secrets manager, or SSM Parameter Store.

---

## Does Terraform support multi-provider deployments?

Yes. Define and use multiple providers in your code.

---

## If I lose the state file in terraform how can I recover that?

- Always use remote state with locking.
- Enable versioning on S3.
- Replicate state files across regions.
- Secure S3 bucket with IAM.
- Enable MFA delete.
- Use monitoring & alerting.
- Restore backup manually if needed.

---

## Null resource in terraform?

Used with provisioners when no actual resource is needed.  
Used for `local-exec`, `remote-exec`, `file`, and `triggers`.

---

## What are modules in terraform?

Modules help reuse code. Benefits:

- Centralised code
- Enforce best practices
- Easy maintenance
- Control and consistency

---

## Is there a tool that can look for security vulnerabilities in your terraform code?

Yes. Examples:

- tflint (used locally)
- argos (considering for CI/CD)

---

## What is the difference between Ansible & Terraform?

- **Terraform**: IaC tool for infra creation (has state).
- **Ansible**: Configuration tool (no state).
- Use both together: Terraform creates, Ansible configures.

---

## Terraform vs Cloudformation?

- **Terraform**: Multi-cloud, state-based.
- **CloudFormation**: AWS only, stack-based (no state file).
- Terraform supports custom and external providers.

---

## I faced one question.. once you created ec2 using terraform, after that you added one tag manually what will terraform do…

- Terraform refreshes the state.
- Detects manual change.
- Removes manual tag to match declared infra on next apply.

---

## How do you deploy your infra code in different environments?

- Use `terraform workspaces`
- Use different `.tfvars` files
- Use same modules, different variable values per env

---

## What are provisioners in terraform

Used to execute scripts after resource creation.

### local-exec

Runs locally:
```hcl
provisioner "local-exec" {
  command = "echo Instance Created: ${self.public_ip} >> instances.txt"
}
```

### remote-exec

Runs on remote server:
```hcl
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
```

### file

Copy files:
```hcl
provisioner "file" {
  source      = "app.conf"
  destination = "/etc/nginx/nginx.conf"
}
```

---

## A Terraform apply fails due to a state file lock issue. How would you resolve it?

- Use `terraform force-unlock <LOCK_ID>`
- Or manually delete lock entry from DynamoDB

---

## Your Terraform deployment created resources in AWS, but some are not being deleted when destroying the infrastructure. What went wrong?

- Resource dependency (e.g., EC2 → SG)
- IAM permissions might prevent deletion
