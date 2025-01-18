Terraform can interact with cloud providers with an API.

### Written Learning Blog
[Terraform Beginner to Pro - Introduction](https://courses.devopsdirective.com/terraform-beginner-to-pro/lessons/00-introduction/01-main)

### Github Credit
[devops-directive-terraform-course](https://github.com/sidpalas/devops-directive-terraform-course)

### Important Points

#### Three Main Approaches for Provisioning Cloud Resources:

- **Cloud Console**: A graphical user interface provided by cloud providers, allowing users to interact with and manage cloud services.
  
- **API or Command-Line Interface**: A method of interacting with cloud services programmatically, allowing for more efficient and automated management.
  
- **Infrastructure as Code**: Defining your entire infrastructure within your codebase, offering better control, visibility, and consistency across environments.

#### Categories of Infrastructure as Code Tools:
- **Ad-hoc scripts**: Basic scripts that make API calls to provision infrastructure resources (e.g., shell scripts).
  
- **Configuration management tools**: Tools like Ansible, Puppet, and Chef, designed to manage software and infrastructure configuration.
  
- **Server templating tools**: Tools for building server templates, such as Amazon Machine Images (AMIs) or virtual machine images.
  
- **Orchestration tools**: Tools like Kubernetes, which focus on deploying applications and managing containers.
  
- **Provisioning tools**: Tools like Terraform, which focus on provisioning cloud resources using a declarative approach.

#### Declarative vs. Imperative:
- **Declarative tools** define the desired end state of your infrastructure (e.g., five servers, one load balancer), and the tool handles the API calls and processes to achieve that state.
  
- **Imperative tools** require you to define the sequence of actions to create the desired infrastructure.

#### Cloud-Specific vs. Cloud-Agnostic:
- **Cloud-specific tools**: These tools, such as AWS CloudFormation or Azure Resource Manager, are provided by major cloud providers and focus on provisioning infrastructure within their respective clouds.
  
- **Cloud-agnostic tools**: Tools like Terraform or Pulumi, which can be used across any cloud provider, allowing for greater flexibility when deploying resources across multiple clouds or when using third-party services.

---

Terraform is a powerful Infrastructure as Code (IaC) tool that allows you to build, change, and version infrastructure safely and easily.

### Features of Terraform:
The cloud-agnostic approach allows for compatibility with multiple cloud providers and services.
- Interact with almost any online service with an API.

Terraform can be used in conjunction with other IaC tools to create powerful and flexible infrastructure management solutions. Some common patterns include:

- **Terraform + Configuration Management Tools (e.g., Ansible)**:
  - Terraform provisions virtual machines
  - Ansible installs and configures dependencies inside virtual machines.

- **Terraform + Orchestration Tools (e.g., Kubernetes)**:
  - Terraform provisions Kubernetes clusters.
  - Kubernetes defines how the application is deployed and managed on the cloud resources.

Here's the updated version of the notes with additional vital points:

## **Terraform Architecture Overview:**
![image](https://github.com/user-attachments/assets/28bb7c91-7b65-4719-ac5b-473db4c4c084)

- **Terraform Core**: 
  - The central engine that manages and controls the provisioning of infrastructure.
  - It processes **configuration files** and the **Terraform state file**.
  - **Configuration files** define the desired infrastructure setup, while the **state file** maintains the current status of the infrastructure already provisioned.
  
- **State Management**:
  - The **state file** is a crucial component as it tracks all the infrastructure Terraform has created or modified. It helps Terraform determine what changes are necessary to match the desired configuration.
  - State can be stored locally or remotely (e.g., in Amazon S3, or Terraform Cloud) for better team collaboration and consistency.
  
- **Providers (Plugins)**: 
  - Providers act as **plugins** to Terraform Core, enabling it to interact with various cloud provider APIs.
  - Each provider understands the configuration format for a particular service (e.g., AWS, Azure, Google Cloud, Cloudflare, etc.).
  - Providers are installed alongside Terraform core, allowing Terraform to authenticate and make requests to the relevant cloud provider API.
  - **There are more than a hundred providers available**, covering nearly all major services and internet resources.
  
- **Execution Plan**:
  - Terraform generates an **execution plan** before applying any changes. This plan is the proposed set of actions Terraform will take to align the infrastructure with the configuration files.
  - The plan is reviewed by the user to ensure that Terraform is taking the correct actions (e.g., adding, modifying, or destroying resources).

- **Resource Provisioning**:
  - **Resources** are the fundamental components managed by Terraform (e.g., servers, databases, networking rules).
  - Terraform ensures the correct provisioning of resources by using the appropriate API calls, as defined by the provider, to create, modify, or destroy resources in the cloud.
  
- **Terraform Modules**:
  - Terraform supports the concept of **modules**, which are reusable configurations that allow for better code organization and reuse across different projects.
  - Modules can contain one or more resources and be shared to ensure consistency in resource management.

- **Terraform CLI**:
  - The **Command-Line Interface (CLI)** is used to interact with Terraform, allowing the user to execute commands like `terraform init`, `terraform plan`, and `terraform apply`.
  
- **State Locking**:
  - When using remote backends for state management, **state locking** is important to ensure that no two users or processes are modifying the same state simultaneously, which prevents conflicts.

- **Cloud-Agnostic Approach**:
  - Terraform’s **cloud-agnostic** nature allows it to work across multiple cloud providers with the same configuration syntax. This flexibility makes it suitable for multi-cloud environments.

### Install and Set Up Terraform with AWS

**Installing Terraform**:  
- For macOS: `brew install terraform`.  
- For other OS, visit HashiCorp’s installation page.

**Authenticating with AWS**:  
- Create an IAM user with necessary permissions (e.g., EC2, RDS, S3, DynamoDB).  
- Install AWS CLI and run `aws configure` to set up credentials.

**Basic Terraform Configuration**:  
Create a `main.tf` file with the following:

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 3.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "example" {
  ami           = "ami-011899242bb902164"
  instance_type = "t2.micro"
}
```

**Terraform Commands**:  
1. `terraform init` – Initialize the project.  
2. `terraform plan` – View the execution plan.  
3. `terraform apply` – Apply changes to create resources.  
4. `terraform destroy` – Clean up resources.

This setup gets Terraform running with AWS and provisions an EC2 instance.
