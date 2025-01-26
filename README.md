# VPC Creation with Terraform & GitHub Actions Automation 
**Introduction**

In modern cloud infrastructure, automating the provisioning of resources is crucial for efficiency and consistency. This project demonstrates how to leverage Terraform, an open-source infrastructure as code (IaC) tool, in conjunction with GitHub Actions, a CI/CD platform, to automate the creation and management of Virtual Private Clouds (VPCs) on cloud platforms

**Prerequisites**
Before embarking on this project, ensure you have the following:

Terraform: Installed on your local machine.
GitHub Account: Access to create repositories and configure actions.
Cloud Provider Account: Credentials for the cloud service where the VPC will be deployed (e.g., AWS, Azure, GCP).

Project Structure
The project is organized as follows:
```bash
project-root/
├── main.tf
├── variables.tf
├── outputs.tf
└── .github/
    └── workflows/
        └── terraform.yml
```
- main.tf: Contains the primary Terraform configuration for the VPC.
- variables.tf: Defines input variables for the Terraform configuration.
- outputs.tf: Specifies the outputs of the Terraform deployment.
- github/workflows/terraform.yml: Houses the GitHub Actions workflow configuration.

** step-by-Step Implementation **
1. Setting Up Terraform
Install Terraform:
Follow the Terraform installation guide to set up Terraform on your local machine.

Create a New Directory:
Create a directory for your project.

bash
Copy
Edit
mkdir terraform-vpc-project
cd terraform-vpc-project
Initialize Terraform:
Run the following command to initialize Terraform:

bash
Copy
Edit
terraform init
2. Writing Terraform Configuration Files
Create the following files in your project directory.

2.1 main.tf
Define the VPC and related resources.

hcl
Copy
Edit
provider "aws" {
  region = var.region
}

resource "aws_vpc" "main" {
  cidr_block = var.vpc_cidr
  tags = {
    Name = var.vpc_name
  }
}

resource "aws_subnet" "subnet" {
  vpc_id                  = aws_vpc.main.id
  cidr_block              = var.subnet_cidr
  map_public_ip_on_launch = true
  tags = {
    Name = "${var.vpc_name}-subnet"
  }
}
2.2 variables.tf
Declare input variables.

hcl
Copy
Edit
variable "region" {
  description = "AWS region"
  default     = "us-east-1"
}

variable "vpc_name" {
  description = "Name of the VPC"
}

variable "vpc_cidr" {
  description = "CIDR block for the VPC"
}

variable "subnet_cidr" {
  description = "CIDR block for the subnet"
}
2.3 outputs.tf
Define output variables.

hcl
Copy
Edit
output "vpc_id" {
  description = "The ID of the VPC"
  value       = aws_vpc.main.id
}

output "subnet_id" {
  description = "The ID of the Subnet"
  value       = aws_subnet.subnet.id
}
3. Configuring GitHub Actions
Create Workflow Directory:
Inside your project repository, create a directory for GitHub Actions workflows.

bash
Copy
Edit
mkdir -p .github/workflows
Add Workflow File:
Create a file named terraform.yml in the .github/workflows directory.

terraform.yml
This file automates Terraform operations via GitHub Actions.

yaml
Copy
Edit
name: Terraform CI/CD

on:
  push:
    branches:
      - main

jobs:
  terraform:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v2
        with:
          terraform_version: 1.5.0

      - name: Terraform Init
        run: terraform init

      - name: Terraform Plan
        run: terraform plan

      - name: Terraform Apply
        run: terraform apply -auto-approve
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
Important:
Store AWS credentials in GitHub Secrets:

AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
4. Running the Workflow
Push Code to GitHub:
Initialize Git and push your project to a GitHub repository.

bash
Copy
Edit
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/your-username/your-repo.git
git push -u origin main
Trigger Workflow:
Each push to the main branch will automatically trigger the workflow.

Monitor Progress:
Navigate to the Actions tab in your GitHub repository to view workflow logs and progress.

Deployment Process
Clone Repository:
Clone your project repository:

bash
Copy
Edit
git clone https://github.com/your-username/your-repo.git
cd your-repo
Review Workflow:
Modify and review Terraform configuration as needed.

Trigger Automation:
Push changes to trigger GitHub Actions for deployment.

Conclusion
This project demonstrates how to automate the provisioning of cloud infrastructure using Terraform and GitHub Actions. By integrating these tools, you can achieve efficient, consistent, and reliable deployments.
