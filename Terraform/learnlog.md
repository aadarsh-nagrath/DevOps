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
- Cloud-agnostic approach allows for compatibility with multiple cloud providers and services.
- Interact with almost any online service with an API.

Terraform can be used in conjunction with other IaC tools to create powerful and flexible infrastructure management solutions. Some common patterns include:

- **Terraform + Configuration Management Tools (e.g., Ansible)**:
  - Terraform provisions virtual machines
  - Ansible installs and configures dependencies inside virtual machines.

- **Terraform + Orchestration Tools (e.g., Kubernetes)**:
  - Terraform provisions Kubernetes clusters.
  - Kubernetes defines how the application is deployed and managed on the cloud resources.
