# Git-Actions-Deployment-Project-Demo

# 🚀 CI/CD Deployment of a Three-Tier Web Application using GitHub Actions and AWS

![GitHub Actions](https://img.shields.io/badge/GitHub-Actions-blue)
![AWS](https://img.shields.io/badge/AWS-Cloud-orange)
![Python](https://img.shields.io/badge/Python-3.12-green)
![Flask](https://img.shields.io/badge/Flask-Backend-lightgrey)
![S3](https://img.shields.io/badge/AWS-S3-red)
![DynamoDB](https://img.shields.io/badge/AWS-DynamoDB-blueviolet)

---

# 📖 Project Overview

This project demonstrates a complete **Continuous Integration and Continuous Deployment (CI/CD)** pipeline for deploying a modern three-tier web application on Amazon Web Services (AWS) using **GitHub Actions**.

Unlike traditional CI/CD pipelines that require dedicated build servers such as Jenkins, this project leverages **GitHub-hosted runners** to automate the build and deployment process. Every time a developer pushes code to GitHub, GitHub Actions automatically deploys the latest version of the application to AWS.

The project is designed as a hands-on laboratory exercise for students to understand modern DevOps practices, cloud deployment, automation, and Infrastructure as Code concepts.

---

# 🎯 Project Objectives

After completing this project, students will be able to:

- Understand Continuous Integration (CI)
- Understand Continuous Deployment (CD)
- Configure GitHub Actions workflows
- Deploy static websites to Amazon S3
- Deploy Flask applications to Amazon EC2
- Configure AWS IAM users and roles
- Store application data in DynamoDB
- Secure deployments using SSH Keys
- Configure GitHub Secrets
- Automate application deployment without Jenkins
- Understand modern DevOps deployment pipelines

---

# 🏗 Project Architecture

```

```
                          Developer
                              │
                              │ Git Push
                              ▼
                     GitHub Repository
                              │
                              ▼
                    GitHub Actions Runner
                     /                     \
                    /                       \
                   ▼                         ▼
        Frontend Workflow            Backend Workflow
               │                            │
               ▼                            ▼
      Amazon S3 Bucket              SSH Deployment
               │                            │
               │                            ▼
               │                      Amazon EC2
               │                            │
               └──────────────► Flask API ◄─┘
                                     │
                                     ▼
                               Amazon DynamoDB

Developer
     │
     ▼
Git Push
     │
     ▼
GitHub Repository
     │
     ▼
GitHub Actions Triggered
     │
     ├─────────────► Frontend Workflow
     │                    │
     │                    ▼
     │              Upload Files to S3
     │
     └─────────────► Backend Workflow
                          │
                          ▼
                    SSH into EC2
                          │
                          ▼
                    Pull Latest Code
                          │
                          ▼
                Install Python Packages
                          │
                          ▼
                 Restart Flask Application

  ✨ Key Features
Fully Automated CI/CD Pipeline
No Jenkins Server Required
GitHub Actions Based Deployment
Automatic Frontend Deployment
Automatic Backend Deployment
Secure SSH Authentication
AWS IAM Integration
Static Website Hosting using Amazon S3
Flask REST API Deployment
DynamoDB Integration
Version Controlled Source Code
GitHub Secrets for Secure Credentials
Beginner Friendly Project Structure
🛠 Technology Stack
Category	Technology
Version Control	Git
Source Repository	GitHub
CI/CD	GitHub Actions
Cloud Provider	AWS
Frontend Hosting	Amazon S3
Backend Hosting	Amazon EC2
Database	Amazon DynamoDB
Programming Language	Python 3.12
Backend Framework	Flask
Web Server	Gunicorn
Authentication	SSH Keys
Operating System	Ubuntu 24.04 LTS
📂 Project Repository Structure

The project is divided into two independent GitHub repositories.

Frontend Repository
assignment-tracker-frontend
│
├── index.html
├── frontend_version.json
└── .github
    └── workflows
        └── frontend.yml
Description
File	Purpose
index.html	Main User Interface
frontend_version.json	Frontend Version Tracking
frontend.yml	GitHub Actions Workflow for S3 Deployment
Backend Repository
assignment-tracker-backend
│
├── app.py
├── requirements.txt
├── backend_version.txt
└── .github
    └── workflows
        └── backend.yml
Description
File	Purpose
app.py	Flask REST API
requirements.txt	Python Dependencies
backend_version.txt	Backend Version
backend.yml	GitHub Actions Workflow
🎓 Learning Outcomes

Upon successful completion of this laboratory exercise, students will be able to:

Build a complete CI/CD pipeline
Deploy applications to AWS
Configure GitHub Actions
Configure IAM Users and Roles
Secure EC2 using SSH Keys
Deploy Flask Applications
Host Static Websites on S3
Connect Applications with DynamoDB
Implement Automated Software Delivery Pipelines
📋 Prerequisites

Before beginning this project, ensure you have the following:

AWS Account
Active AWS Account
Administrator or equivalent permissions
GitHub Account
Active GitHub Account
Basic Git Knowledge
Knowledge Requirements

Students should be familiar with:

Basic Linux Commands
Git and GitHub
Python Basics
Cloud Computing Fundamentals
AWS Console Navigation
☁ AWS Services Used

The following AWS services are used throughout this project.

AWS Service	Purpose
Amazon EC2	Host Flask Backend
Amazon S3	Static Website Hosting
Amazon IAM	Authentication & Authorization
Amazon DynamoDB	NoSQL Database
🔐 Software Requirements

Install the following software before starting the project.

Software	Version
Git	Latest
Python	3.12
VS Code	Latest
Google Chrome	Latest
AWS CLI	Latest
OpenSSH	Latest
📌 Deployment Strategy

This project follows the Git Push → Automatic Deployment strategy.

Whenever code is pushed to the GitHub repository:

GitHub detects the new commit.
GitHub Actions automatically starts.
The frontend workflow uploads files to Amazon S3.
The backend workflow connects to EC2 using SSH.
The latest backend code is pulled.
Python dependencies are installed.
The Flask application is restarted.
The latest version becomes available to users automatically.

No manual deployment is required.

🚀 Part 2:
includs:

Amazon S3 Bucket
Backend EC2 Instance
DynamoDB Tables
IAM Role Configuration
Backend Server Preparation
Python Environment Setup
Initial Server Configuration

# Part 2: AWS Infrastructure Setup

Before configuring the CI/CD pipeline, we must provision the AWS infrastructure that will host our application. This project uses a three-tier architecture consisting of:

- **Presentation Layer** – Amazon S3 Static Website
- **Application Layer** – Amazon EC2 running a Flask application
- **Data Layer** – Amazon DynamoDB

The CI/CD pipeline created in later sections will automatically deploy the application to this infrastructure.

---

# AWS Infrastructure Overview

| Layer | AWS Service | Purpose |
|--------|-------------|---------|
| Presentation Layer | Amazon S3 | Hosts the static frontend website |
| Application Layer | Amazon EC2 | Runs the Flask backend application |
| Data Layer | Amazon DynamoDB | Stores assignments and submissions |
| Identity Management | AWS IAM | Secure access to AWS resources |

---

# Step 1: Create the Frontend S3 Bucket

The frontend of this application is a static website consisting of HTML, CSS, JavaScript, and other static assets. Amazon S3 provides an inexpensive and highly available platform for hosting such websites.

---

## Step 1.1 Open Amazon S3

1. Sign in to the AWS Management Console.
2. Search for **S3**.
3. Open the **Amazon S3 Console**.
4. Click **Create Bucket**.

---

## Step 1.2 Configure the Bucket

Enter the following details.

| Property | Value |
|----------|-------|
| Bucket Name | assignment-tracker-frontend |
| AWS Region | us-east-1 (N. Virginia) |
| Object Ownership | ACLs Disabled (Default) |
| Block Public Access | Uncheck "Block all public access" |

When disabling public access, AWS displays a warning.

Enable the confirmation checkbox acknowledging that the bucket will be publicly accessible.

> **Note**
>
> Public access is enabled only because this project hosts a static website. For production environments, it is recommended to use Amazon CloudFront with Origin Access Control (OAC) instead of exposing the bucket directly.

Click **Create Bucket**.

---

## Step 1.3 Enable Static Website Hosting

Open the newly created bucket.

Navigate to:

```
Properties
```

Scroll down to:

```
Static Website Hosting
```

Click:

```
Edit
```

Configure:

| Property | Value |
|----------|-------|
| Static Website Hosting | Enable |
| Hosting Type | Host a Static Website |
| Index Document | index.html |
| Error Document | index.html (optional) |

Click **Save Changes**.

---

## Step 1.4 Configure Bucket Policy

Open

```
Permissions
```

Scroll to

```
Bucket Policy
```

Click

```
Edit
```

Replace the bucket name in the following policy.

```json
{
  "Version":"2012-10-17",
  "Statement":[
    {
      "Sid":"PublicReadGetObject",
      "Effect":"Allow",
      "Principal":"*",
      "Action":"s3:GetObject",
      "Resource":"arn:aws:s3:::assignment-tracker-frontend/*"
    }
  ]
}
```

Save the policy.

---

## Step 1.5 Verify the Website Endpoint

Return to

```
Properties
```

Locate

```
Static Website Hosting
```

You will find a Website Endpoint similar to:

```
http://assignment-tracker-frontend.s3-website-us-east-1.amazonaws.com
```

Keep this URL safe.

It will become the public URL of the frontend after deployment.

---

# Step 2: Launch the Backend EC2 Instance

The backend of this project is developed using Flask and will run continuously on an Ubuntu EC2 instance.

---

## Step 2.1 Launch Instance

Open

```
Amazon EC2
```

Click

```
Launch Instance
```

Use the following configuration.

| Property | Value |
|----------|-------|
| Name | assignment-tracker-backend |
| AMI | Ubuntu Server 24.04 LTS |
| Instance Type | t3.small |
| Storage | 20 GB |
| Key Pair | Existing or New Key Pair |

---

## Step 2.2 Configure Security Group

Create a new security group.

Allow the following inbound rules.

| Port | Protocol | Purpose |
|------|----------|----------|
| 22 | SSH | Remote Administration |
| 5000 | TCP | Flask Backend API |

Example

| Type | Port | Source |
|------|------|--------|
| SSH | 22 | Anywhere |
| Custom TCP | 5000 | Anywhere |

> **Important**
>
> Opening ports to Anywhere (0.0.0.0/0) is acceptable for learning purposes only.
> In production environments, inbound access should always be restricted.

Launch the instance.

---

## Step 2.3 Note the Public IPv4 Address

Once the instance reaches the **Running** state,

copy the

```
Public IPv4 Address
```

Example

```
54.xx.xx.xx
```

This address will later be stored as a GitHub Secret.

---

# Step 3: Create DynamoDB Tables

The application stores data inside Amazon DynamoDB.

Two tables are required.

---

## Table 1 — ASSIGNMENTS

Navigate to

```
Amazon DynamoDB
```

Click

```
Create Table
```

Configure

| Property | Value |
|----------|-------|
| Table Name | ASSIGNMENTS |
| Partition Key | assignment_id |
| Key Type | String |

Leave all remaining options as default.

Click

```
Create Table
```

---

## Table 2 — SUBMISSIONS

Again click

```
Create Table
```

Configure

| Property | Value |
|----------|-------|
| Table Name | SUBMISSIONS |
| Partition Key | submission_id |
| Key Type | String |

Leave all remaining settings unchanged.

Click

```
Create Table
```

After a few moments both tables should display the status:

```
Active
```

---

# Step 4: Configure IAM Role for Backend EC2

The Flask application requires permission to access DynamoDB and Amazon S3.

Instead of embedding AWS credentials inside the application, AWS recommends assigning an IAM Role to the EC2 instance.

---

## Step 4.1 Create IAM Role

Navigate to

```
IAM
```

Select

```
Roles
```

Click

```
Create Role
```

Choose

```
Trusted Entity
EC2
```

Click

```
Next
```

Attach the following policies.

| Policy |
|----------|
| AmazonDynamoDBFullAccess |
| AmazonS3FullAccess |

Click

```
Next
```

Role Name

```
backend-server-role
```

Click

```
Create Role
```

---

## Step 4.2 Attach Role to EC2

Navigate to

```
EC2
```

Select

```
assignment-tracker-backend
```

Choose

```
Actions
```

```
Security
```

```
Modify IAM Role
```

Select

```
backend-server-role
```

Click

```
Update IAM Role
```

---

## Step 4.3 Verify the IAM Role

SSH into the server.

Run

```bash
aws sts get-caller-identity
```

Expected Output

```json
{
  "Account": "xxxxxxxxxxxx",
  "Arn": "arn:aws:sts::xxxxxxxxxxxx:assumed-role/backend-server-role/i-xxxxxxxx"
}
```

If this command succeeds, the EC2 instance can securely access AWS services without storing access keys.

---

# Step 5: Prepare the Backend Server

Connect to the EC2 instance using SSH.

```bash
ssh -i key.pem ubuntu@<EC2-Public-IP>
```

---

## Update Ubuntu

```bash
sudo apt update
sudo apt upgrade -y
```

---

## Install Git

```bash
sudo apt install git -y
```

Verify

```bash
git --version
```

---

## Install Python

```bash
sudo apt install python3-pip -y
```

Verify

```bash
python3 --version
```

---

## Install Python Virtual Environment

```bash
sudo apt install python3.12-venv -y
```

Verify

```bash
python3 -m venv --help
```

---

## Create the Deployment Directory

All application files will be deployed inside the following directory.

```bash
mkdir ~/app
```

Verify

```bash
ls
```

You should see

```
app
```

---

# Infrastructure Checklist

Before moving to the next section, ensure that all infrastructure components are ready.

| Component | Status |
|-----------|--------|
| S3 Bucket Created | ✅ |
| Static Website Hosting Enabled | ✅ |
| Bucket Policy Applied | ✅ |
| Backend EC2 Running | ✅ |
| Security Group Configured | ✅ |
| DynamoDB Tables Created | ✅ |
| IAM Role Created | ✅ |
| IAM Role Attached to EC2 | ✅ |
| Git Installed | ✅ |
| Python Installed | ✅ |
| Python Virtual Environment Installed | ✅ |
| Deployment Directory Created | ✅ |

---

🚀 Part 3A:

# Part 3A: GitHub Repository Setup and SSH Configuration

In this section, we will prepare GitHub for automated deployments using GitHub Actions. The following tasks will be completed:

- Generate SSH keys on the Backend EC2 instance
- Configure passwordless SSH authentication
- Create GitHub repositories
- Download and organize the project source code
- Configure Git
- Push the initial source code to GitHub

After completing this section, your project source code will be securely stored in GitHub and ready for CI/CD automation.

---

# Step 6: Configure SSH Authentication for GitHub Actions

GitHub Actions deploys the backend application by connecting to the EC2 instance through SSH.

Instead of storing an EC2 password, we will use **public-key authentication**, which is the recommended and secure method for automated deployments.

## Understanding SSH Keys

SSH authentication uses two keys:

| Key | Purpose | Keep Secret? |
|------|----------|--------------|
| Private Key (`id_ed25519`) | Used by GitHub Actions to log in to EC2 | ✅ Yes |
| Public Key (`id_ed25519.pub`) | Stored on the EC2 server to authorize logins | ❌ No |

> **Important**
>
> Never share your private key with anyone or commit it to a GitHub repository.

---

## Step 6.1 Connect to the Backend EC2 Instance

From your local computer, connect to the EC2 instance.

```bash
ssh -i key.pem ubuntu@<EC2-PUBLIC-IP>
```

Replace `<EC2-PUBLIC-IP>` with your server's public IP address.

Example:

```bash
ssh -i assignment-key.pem ubuntu@54.196.25.120
```

---

## Step 6.2 Generate an SSH Key Pair

Run the following command:

```bash
ssh-keygen -t ed25519 -C "github-actions"
```

You will be prompted with the following questions:

```
Enter file in which to save the key:
```

Press **Enter** to accept the default location.

```
Enter passphrase:
```

Press **Enter**.

```
Enter same passphrase again:
```

Press **Enter**.

---

## Step 6.3 Verify the Generated Keys

List the contents of the `.ssh` directory:

```bash
ls -la ~/.ssh
```

Expected output:

```
authorized_keys
id_ed25519
id_ed25519.pub
known_hosts
```

---

## Step 6.4 Understand the Generated Files

| File | Description |
|------|-------------|
| id_ed25519 | Private SSH Key |
| id_ed25519.pub | Public SSH Key |
| authorized_keys | Stores trusted public keys |
| known_hosts | Stores information about previously connected hosts |

---

## Step 6.5 Authorize the Public Key

Append the generated public key to the `authorized_keys` file:

```bash
cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys
```

---

## Step 6.6 Set Correct Permissions

Run the following commands:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

These permissions are required for OpenSSH to accept key-based authentication.

---

## Step 6.7 Display the Private Key

GitHub Actions requires the private key.

Display it using:

```bash
cat ~/.ssh/id_ed25519
```

Example:

```
-----BEGIN OPENSSH PRIVATE KEY-----

b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAA...

-----END OPENSSH PRIVATE KEY-----
```

> **Important**
>
> Copy the **entire contents**, including:
>
> - `BEGIN OPENSSH PRIVATE KEY`
> - `END OPENSSH PRIVATE KEY`

This key will be added later as a GitHub Secret.

---

# Step 7: Create GitHub Repositories

The project consists of two independent repositories.

| Repository | Purpose |
|------------|---------|
| assignment-tracker-frontend | Static Website |
| assignment-tracker-backend | Flask Application |

---

## Step 7.1 Create the Frontend Repository

1. Open GitHub.
2. Click **New Repository**.
3. Configure:

| Property | Value |
|----------|-------|
| Repository Name | assignment-tracker-frontend |
| Visibility | Public (Workshop) or Private |
| Initialize with README | No |
| Add .gitignore | No |
| License | None |

Click **Create Repository**.

---

## Step 7.2 Create the Backend Repository

Repeat the same process.

Repository Name:

```
assignment-tracker-backend
```

---

# Step 8: Configure Git on Your Local Machine

Before pushing code to GitHub, configure Git with your identity.

```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

Verify:

```bash
git config --list
```

Expected output:

```
user.name=Your Name
user.email=your-email@example.com
```

---

# Step 9: Clone the GitHub Repositories

Clone the newly created repositories.

## Frontend Repository

```bash
git clone https://github.com/<YOUR_GITHUB_USERNAME>/assignment-tracker-frontend.git
```

## Backend Repository

```bash
git clone https://github.com/<YOUR_GITHUB_USERNAME>/assignment-tracker-backend.git
```

Example directory structure:

```
Projects
│
├── assignment-tracker-frontend
│
└── assignment-tracker-backend
```

---

# Step 10: Download the Project Source Code

The application source code is available in the project repository.

### Frontend Files

Copy the following files into the **assignment-tracker-frontend** repository.

```
index.html
frontend_version.json
```

---

### Backend Files

Copy the following files into the **assignment-tracker-backend** repository.

```
app.py
requirements.txt
backend_version.txt
```

---

## Repository Structure

### Frontend

```
assignment-tracker-frontend
│
├── index.html
├── frontend_version.json
└── .github
```

---

### Backend

```
assignment-tracker-backend
│
├── app.py
├── requirements.txt
├── backend_version.txt
└── .github
```

---

# Step 11: Verify the Project Files

Verify the contents of each repository.

Frontend:

```bash
cd assignment-tracker-frontend
ls
```

Expected output:

```
index.html
frontend_version.json
```

Backend:

```bash
cd assignment-tracker-backend
ls
```

Expected output:

```
app.py
requirements.txt
backend_version.txt
```

---

# Step 12: Commit the Source Code

## Frontend Repository

```bash
cd assignment-tracker-frontend

git add .

git commit -m "Initial frontend project"
```

---

## Backend Repository

```bash
cd assignment-tracker-backend

git add .

git commit -m "Initial backend project"
```

---

# Step 13: Push the Source Code

Push the repositories to GitHub.

## Frontend

```bash
git push origin main
```

---

## Backend

```bash
git push origin main
```

If prompted, authenticate using your GitHub credentials or Personal Access Token (PAT).

---

# Step 14: Verify the GitHub Repositories

Open your repositories in a web browser.

Frontend:

```
https://github.com/<YOUR_GITHUB_USERNAME>/assignment-tracker-frontend
```

Backend:

```
https://github.com/<YOUR_GITHUB_USERNAME>/assignment-tracker-backend
```

Confirm that the files have been uploaded successfully.

🚀 Part 3B:

# Part 3B: Configure GitHub Secrets and AWS IAM User

In the previous section, we prepared the GitHub repositories and uploaded the application source code.

In this section, we will configure the authentication required by GitHub Actions to automatically deploy the application.

By the end of this section, GitHub Actions will have secure access to:

- Amazon S3 (Frontend Deployment)
- Amazon EC2 (Backend Deployment)

---

# Why are GitHub Secrets Required?

GitHub Actions executes workflows on GitHub-hosted runners.

These runners need credentials to access AWS resources and connect to the backend EC2 instance.

Instead of storing passwords or access keys inside the workflow file, GitHub provides **Repository Secrets**, which are encrypted and securely injected into workflows during execution.

> **Important**
>
> Never hardcode AWS credentials, SSH keys, passwords, or API keys inside your source code or workflow files.

---

# Authentication Overview

The project uses two different authentication mechanisms.

| Deployment | Authentication Method |
|------------|-----------------------|
| Frontend → Amazon S3 | AWS Access Key + Secret Key |
| Backend → Amazon EC2 | SSH Private Key |

---

# Step 14: Create an IAM User for GitHub Actions

GitHub Actions requires permission to upload frontend files to the Amazon S3 bucket.

Instead of using the AWS root account, we will create a dedicated IAM user following AWS security best practices.

---

## Step 14.1 Open IAM Console

1. Sign in to the AWS Console.
2. Search for **IAM**.
3. Open the IAM Dashboard.
4. Select **Users** from the left navigation panel.
5. Click **Create User**.

---

## Step 14.2 Configure the IAM User

Enter the following details.

| Property | Value |
|----------|-------|
| User Name | github-actions-user |

Leave all other options as default.

Click **Next**.

---

## Step 14.3 Assign Permissions

Choose

```
Attach Policies Directly
```

Search and attach the following policy.

```
AmazonS3FullAccess
```

Click

```
Next
```

Review the configuration.

Click

```
Create User
```

---

# Step 15: Generate AWS Access Keys

The IAM user must have an Access Key so GitHub Actions can authenticate with AWS.

---

## Step 15.1 Open Security Credentials

Select the newly created IAM user.

Navigate to

```
Security Credentials
```

Scroll down to

```
Access Keys
```

Click

```
Create Access Key
```

---

## Step 15.2 Select Use Case

AWS displays multiple options.

Select

```
Command Line Interface (CLI)
```

Click

```
Next
```

Optionally provide a description.

Example

```
GitHub Actions Deployment
```

Click

```
Create Access Key
```

---

## Step 15.3 Save the Credentials

AWS displays two values.

Example

```
AWS_ACCESS_KEY_ID

AKIAxxxxxxxxxxxxxxxx
```

and

```
AWS_SECRET_ACCESS_KEY

AbCdEf123456789xxxxxxxxxxxxxxxx
```

> **Important**
>
> AWS displays the Secret Access Key only once.
>
> Store it safely before closing the page.

---

# Step 16: Configure Backend Repository Secrets

The backend workflow connects to the EC2 instance through SSH.

Three repository secrets are required.

Open your backend repository.

```
assignment-tracker-backend
```

Navigate to

```
Settings
```

↓

```
Secrets and variables
```

↓

```
Actions
```

Click

```
New repository secret
```

---

## Secret 1: EC2_HOST

This is the Public IPv4 Address of the backend EC2 instance.

Example

```
54.196.25.120
```

Create the secret.

| Name | Value |
|------|-------|
| EC2_HOST | 54.196.25.120 |

---

## Secret 2: EC2_USER

This is the SSH username of your EC2 instance.

For Ubuntu 24.04

```
ubuntu
```

| Name | Value |
|------|-------|
| EC2_USER | ubuntu |

---

## Step 16.3 Secret 3: EC2_SSH_KEY

This is the most important secret.

GitHub Actions requires the **private SSH key**, **not** the public key.

Open the backend EC2 terminal.

Display the private key.

```bash
cat ~/.ssh/id_ed25519
```

Example

```
-----BEGIN OPENSSH PRIVATE KEY-----

b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAA...

-----END OPENSSH PRIVATE KEY-----
```

Copy the **entire content**, including

```
BEGIN OPENSSH PRIVATE KEY
```

and

```
END OPENSSH PRIVATE KEY
```

Create the secret.

| Name | Value |
|------|-------|
| EC2_SSH_KEY | Entire private key |

---

## Backend Secrets Summary

Your backend repository should now contain the following secrets.

| Secret | Example |
|---------|---------|
| EC2_HOST | 54.xx.xx.xx |
| EC2_USER | ubuntu |
| EC2_SSH_KEY | Entire OpenSSH Private Key |

---

# Step 17: Configure Frontend Repository Secrets

Open

```
assignment-tracker-frontend
```

Navigate to

```
Settings
```

↓

```
Secrets and variables
```

↓

```
Actions
```

Click

```
New repository secret
```

Create the following four secrets.

---

## Secret 1: AWS_ACCESS_KEY_ID

Use the Access Key generated for the IAM user.

Example

```
AKIAxxxxxxxxxxxxxxxx
```

---

## Secret 2: AWS_SECRET_ACCESS_KEY

Use the Secret Access Key generated by AWS.

Example

```
AbCdEf123456789xxxxxxxxxxxxxx
```

---

## Secret 3: AWS_REGION

Specify the AWS Region where the S3 bucket was created.

Example

```
us-east-1
```

---

## Secret 4: S3_BUCKET_NAME

Enter only the bucket name.

Correct

```
assignment-tracker-frontend
```

Incorrect

```
s3://assignment-tracker-frontend
```

---

## Frontend Secrets Summary

| Secret | Example |
|---------|---------|
| AWS_ACCESS_KEY_ID | AKIAxxxxxxxx |
| AWS_SECRET_ACCESS_KEY | **************** |
| AWS_REGION | us-east-1 |
| S3_BUCKET_NAME | assignment-tracker-frontend |

---

# Step 18: How GitHub Uses Repository Secrets

During workflow execution, GitHub automatically injects repository secrets as environment variables.

Example

```yaml
env:
  EC2_HOST: ${{ secrets.EC2_HOST }}
  EC2_USER: ${{ secrets.EC2_USER }}
```

Similarly,

```yaml
env:
  AWS_REGION: ${{ secrets.AWS_REGION }}
```

The actual values remain encrypted and are never displayed in the repository.

---

# Security Best Practices

Always follow these guidelines while working with GitHub Actions.

- Never commit AWS credentials into GitHub.
- Never commit private SSH keys.
- Never share repository secrets.
- Rotate AWS Access Keys periodically.
- Use IAM Roles whenever possible.
- Grant only the minimum permissions required.
- Remove unused credentials immediately.
- Store sensitive information only in GitHub Secrets.

---

# Verify Repository Secrets

Before proceeding, verify that both repositories contain the required secrets.

## Backend Repository

| Secret | Configured |
|---------|------------|
| EC2_HOST | ✅ |
| EC2_USER | ✅ |
| EC2_SSH_KEY | ✅ |

---

## Frontend Repository

| Secret | Configured |
|---------|------------|
| AWS_ACCESS_KEY_ID | ✅ |
| AWS_SECRET_ACCESS_KEY | ✅ |
| AWS_REGION | ✅ |
| S3_BUCKET_NAME | ✅ |

---

# Final Readiness Checklist

Before creating GitHub Actions workflows, ensure that all the following tasks have been completed.

| Task | Status |
|------|--------|
| Backend EC2 Running | ✅ |
| S3 Bucket Created | ✅ |
| DynamoDB Tables Created | ✅ |
| IAM Role Attached to EC2 | ✅ |
| SSH Key Pair Generated | ✅ |
| GitHub Repositories Created | ✅ |
| Source Code Uploaded | ✅ |
| IAM User Created | ✅ |
| AWS Access Keys Generated | ✅ |
| Backend Secrets Configured | ✅ |
| Frontend Secrets Configured | ✅ |

---

# Architecture After Completing Part 3

```
                        Developer
                             │
                             ▼
                     GitHub Repository
                             │
                             │
                   Repository Secrets
                             │
          ┌──────────────────┴──────────────────┐
          │                                     │
          ▼                                     ▼
Frontend Repository                    Backend Repository
          │                                     │
 AWS Credentials                     SSH Private Key
          │                                     │
          └──────────────┬──────────────────────┘
                         │
                Ready for GitHub Actions
```

---

# What's Next?

Congratulations! 🎉

At this stage:

- AWS Infrastructure has been configured.
- Backend EC2 is ready.
- Amazon S3 is ready.
- DynamoDB tables are active.
- GitHub repositories have been created.
- Source code has been uploaded.
- Repository Secrets have been configured.

The project is now fully prepared for **GitHub Actions**.

🚀 Part 4:
     Section 4.1:
     # Part 4A.1: Introduction to GitHub Actions and CI/CD Architecture

---

# Introduction

In the previous sections of this workshop, we successfully completed the following tasks:

* Created the AWS infrastructure.
* Configured the Amazon S3 bucket for hosting the frontend.
* Launched and configured the Backend EC2 instance.
* Created the DynamoDB tables.
* Configured IAM Roles and IAM Users.
* Generated SSH keys for secure authentication.
* Created GitHub repositories.
* Uploaded the application source code.
* Configured GitHub Repository Secrets.

At this stage, the complete infrastructure is ready.

The only missing component is **automation**.

Without automation, every modification to the application requires the developer to manually:

* Upload frontend files to Amazon S3.
* Connect to the EC2 instance using SSH.
* Pull the latest source code.
* Install required Python packages.
* Restart the Flask application.

These repetitive tasks consume time, increase the possibility of human error, and become impractical as projects grow.

To solve this problem, we use **Continuous Integration and Continuous Deployment (CI/CD)**.

---

# What is CI/CD?

CI/CD stands for **Continuous Integration** and **Continuous Deployment**.

It is a software engineering practice that automates the process of integrating code changes, testing applications, and deploying software to production environments.

Instead of manually deploying every code change, a CI/CD pipeline performs these tasks automatically whenever a developer pushes new code to the repository.

---

## Continuous Integration (CI)

Continuous Integration is the practice of regularly merging source code changes into a central repository.

Whenever developers push new code:

* The repository detects the change.
* An automated workflow starts.
* The latest code is downloaded.
* Build and validation tasks are executed.
* Any errors are reported immediately.

### Benefits of Continuous Integration

* Detects problems early.
* Prevents integration conflicts.
* Reduces manual work.
* Improves collaboration among developers.
* Ensures that the repository always contains a working version of the application.

---

## Continuous Deployment (CD)

Continuous Deployment automatically delivers the latest version of the application after successful integration.

Instead of manually copying files to servers, the deployment process is fully automated.

Typical deployment tasks include:

* Uploading frontend files.
* Updating backend source code.
* Installing dependencies.
* Restarting application services.
* Making the latest version available to end users.

---

# Traditional Deployment Process

Without CI/CD, a developer typically performs the following sequence of tasks.

```text
Developer
     │
     ▼
Modify Source Code
     │
     ▼
Manual Git Push
     │
     ▼
Login to AWS Console
     │
     ▼
Upload Frontend Files
     │
     ▼
SSH into EC2
     │
     ▼
Pull Latest Code
     │
     ▼
Install Dependencies
     │
     ▼
Restart Application
```

This approach is:

* Time-consuming
* Error-prone
* Difficult to maintain
* Not scalable

---

# Automated Deployment Process

With GitHub Actions, the deployment becomes fully automated.

```text
Developer
      │
      ▼
Modify Source Code
      │
      ▼
Git Push
      │
      ▼
GitHub Actions
      │
      ▼
Automatic Deployment
```

The developer only writes code.

Everything else is handled automatically.

---

# Why GitHub Actions?

GitHub Actions is GitHub's native CI/CD platform.

It allows workflows to be executed directly inside a GitHub repository without installing any additional software.

Whenever an event occurs inside a repository, GitHub can automatically execute predefined workflows.

Common events include:

* Code Push
* Pull Request
* Release
* Tag Creation
* Scheduled Jobs
* Manual Trigger

---

# Why GitHub Actions Instead of Jenkins?

Historically, Jenkins has been the most widely used CI/CD tool.

However, modern software development increasingly favors GitHub Actions because it eliminates the need to maintain a dedicated build server.

The following table compares both approaches.

| Feature                   | Jenkins   | GitHub Actions |
| ------------------------- | --------- | -------------- |
| Installation Required     | Yes       | No             |
| Build Server              | Required  | Not Required   |
| Plugin Management         | Required  | Minimal        |
| Maintenance               | High      | Very Low       |
| Native GitHub Integration | Limited   | Excellent      |
| Workflow as Code          | Supported | Supported      |
| Cloud Hosted Runner       | No        | Yes            |
| Cost for Small Projects   | Higher    | Lower          |

For educational projects and small to medium-sized applications, GitHub Actions provides a simpler and more modern solution.

---

# GitHub Actions Architecture

The architecture used in this workshop is illustrated below.

```text
                    Developer
                         │
                         │ Git Push
                         ▼
               GitHub Repository
                         │
                         ▼
               GitHub Actions Runner
                  ┌───────────────┐
                  │               │
                  ▼               ▼
         Frontend Workflow   Backend Workflow
                  │               │
                  ▼               ▼
             Amazon S3       SSH into EC2
                                  │
                                  ▼
                           Flask Application
                                  │
                                  ▼
                            Amazon DynamoDB
```

---

# Workflow Execution Sequence

Whenever a developer pushes new code, the following sequence takes place.

```text
Developer
      │
      ▼
Git Push
      │
      ▼
GitHub Repository
      │
      ▼
GitHub detects workflow file
      │
      ▼
Creates Ubuntu Runner
      │
      ▼
Downloads Repository
      │
      ▼
Executes Workflow
      │
      ▼
Deploys Application
```

The entire process is automatic.

No manual intervention is required.

---

# Understanding the GitHub Runner

One of the most important concepts in GitHub Actions is the **Runner**.

A Runner is a temporary virtual machine that GitHub creates whenever a workflow starts.

Think of it as a short-lived computer provided by GitHub specifically for executing your workflow.

During execution, the Runner performs tasks such as:

* Downloading the repository.
* Executing shell commands.
* Installing software if required.
* Connecting to AWS.
* Deploying the application.

Once the workflow finishes, the Runner is destroyed automatically.

A new Runner is created for every workflow execution.

---

# GitHub Hosted Runner

In this workshop, we use a GitHub-hosted Ubuntu Runner.

```yaml
runs-on: ubuntu-latest
```

This means GitHub automatically creates an Ubuntu virtual machine every time the workflow runs.

No EC2 instance is required for building the application.

---

# Complete Deployment Architecture

The following diagram summarizes the complete deployment pipeline implemented in this project.

```text
                  Developer
                       │
                       ▼
                 Source Code
                       │
                       ▼
               GitHub Repository
                       │
          Push Event Triggered
                       │
                       ▼
              GitHub Actions Runner
               ┌─────────────────┐
               │                 │
               ▼                 ▼
        Frontend Workflow   Backend Workflow
               │                 │
               ▼                 ▼
         Amazon S3 Bucket     SSH Deployment
                                 │
                                 ▼
                           Backend EC2
                                 │
                                 ▼
                           Flask Application
                                 │
                                 ▼
                          Amazon DynamoDB
```

---

# Advantages of This Architecture

This architecture provides several benefits.

### Automation

Every deployment is automatic.

### Reliability

Manual deployment mistakes are eliminated.

### Faster Releases

Developers only need to push code.

### Version Control

Every deployment corresponds to a Git commit.

### Security

Sensitive credentials remain encrypted using GitHub Secrets.

### Scalability

The same workflow can be extended to larger production environments.

---

# Industry Insight

In enterprise environments, the same architecture is often extended with additional AWS services such as:

* Elastic Load Balancer (ELB)
* Auto Scaling Groups
* Amazon CloudFront
* Amazon Route 53
* AWS CodeDeploy
* Amazon ECS
* Amazon EKS
* AWS Systems Manager

Although our workshop uses a simplified architecture, the deployment principles remain the same.

    Section 4.2:
    # Part 4A.2: GitHub Actions Fundamentals

---

# Introduction

In the previous section, we learned that GitHub Actions automates software deployment whenever code is pushed to a GitHub repository.

Before creating our first workflow, it is important to understand the core building blocks of GitHub Actions.

Think of GitHub Actions as a programming language for automation.

Just as a programming language has variables, functions, loops, and conditions, GitHub Actions has its own components:

* Workflow
* Event
* Job
* Step
* Action
* Runner
* Secrets
* Environment Variables

Once you understand these concepts, writing workflow files becomes straightforward.

---

# GitHub Actions Hierarchy

The following diagram illustrates how GitHub Actions is organized.

```text
Workflow
│
├── Job 1
│      ├── Step 1
│      ├── Step 2
│      └── Step 3
│
└── Job 2
       ├── Step 1
       ├── Step 2
       └── Step 3
```

Everything begins with a **Workflow**.

A workflow contains one or more **Jobs**.

Each job contains one or more **Steps**.

Each step executes a command or an action.

---

# What is a Workflow?

A **Workflow** is an automated process that GitHub executes whenever a specific event occurs.

A workflow is stored as a YAML file inside the repository.

Location:

```text
.github/
    workflows/
```

Example:

```text
.github/
    workflows/
        frontend.yml
```

or

```text
.github/
    workflows/
        backend.yml
```

GitHub automatically scans this folder whenever code is pushed.

If it finds workflow files, it executes them according to the defined trigger.

---

# Why Must Workflow Files Be Stored Here?

GitHub only recognizes workflow files located in:

```text
.github/workflows/
```

If a workflow is stored elsewhere, GitHub completely ignores it.

For example:

✅ Correct

```text
.github/workflows/frontend.yml
```

❌ Incorrect

```text
workflow/frontend.yml
```

❌ Incorrect

```text
.github/frontend.yml
```

---

# What is YAML?

GitHub Actions workflows are written using **YAML**.

YAML stands for:

> **YAML Ain't Markup Language**

YAML is a human-readable format commonly used for configuration files.

Example:

```yaml
name: Frontend Deployment

on:
  push:
    branches:
      - main
```

Notice that YAML uses **indentation** instead of braces `{}` or semicolons `;`.

> **Important**
>
> Indentation is mandatory in YAML.
> Incorrect spacing will cause the workflow to fail.

---

# What is an Event?

An **Event** is an activity that triggers a workflow.

Examples include:

* Code Push
* Pull Request
* Release Creation
* Issue Creation
* Manual Trigger
* Scheduled Execution

In this project, we use the **push** event.

Example:

```yaml
on:
  push:
    branches:
      - main
```

This means:

> Whenever code is pushed to the **main** branch, execute the workflow.

---

# Event Flow

```text
Developer
      │
      ▼
Git Push
      │
      ▼
GitHub detects Push Event
      │
      ▼
Workflow Starts
```

---

# What is a Job?

A **Job** is a collection of related tasks.

Each workflow contains one or more jobs.

Example:

```yaml
jobs:

  deploy:
```

Here,

```
deploy
```

is the job name.

A job executes on a GitHub Runner.

---

# Job Execution

```text
Workflow
     │
     ▼
Job
     │
     ▼
Runner
     │
     ▼
Steps
```

A workflow can contain multiple jobs.

Example

```text
Workflow

├── Build

├── Test

└── Deploy
```

Jobs can run

* Sequentially
* In Parallel

depending on the workflow configuration.

---

# What is a Step?

A **Step** is a single instruction executed inside a Job.

Examples:

* Checkout repository
* Install Python
* Upload files
* Execute shell commands
* Restart server

Example:

```yaml
steps:

- name: Checkout Repository

- name: Configure AWS

- name: Upload Files
```

Each step executes one task.

---

# Step Execution

```text
Job

│

├── Step 1

├── Step 2

├── Step 3

└── Step 4
```

Steps always execute in order unless explicitly configured otherwise.

---

# What is an Action?

An **Action** is a reusable component created by GitHub or the open-source community.

Instead of writing lengthy scripts, you can simply reuse existing actions.

Example:

```yaml
uses: actions/checkout@v4
```

This action automatically downloads the repository.

Another example:

```yaml
uses: aws-actions/configure-aws-credentials@v4
```

This action automatically authenticates with AWS.

Actions help reduce development time and improve workflow readability.

---

# GitHub Marketplace

GitHub provides thousands of reusable actions through the GitHub Marketplace.

Popular actions include:

| Action                    | Purpose                 |
| ------------------------- | ----------------------- |
| actions/checkout          | Download repository     |
| configure-aws-credentials | Login to AWS            |
| setup-python              | Install Python          |
| upload-artifact           | Save build files        |
| appleboy/ssh-action       | SSH into remote servers |

---

# What is a Runner?

A Runner is the computer that executes the workflow.

GitHub supports two types of runners.

## GitHub Hosted Runner

Provided and managed by GitHub.

Example

```yaml
runs-on: ubuntu-latest
```

GitHub automatically creates

* Ubuntu VM
* Downloads repository
* Executes workflow
* Deletes VM after completion

No maintenance is required.

---

## Self-Hosted Runner

Instead of using GitHub's infrastructure, organizations can execute workflows on their own servers.

Example

```text
Company Server
       │
       ▼
Self Hosted Runner
```

Large enterprises often use self-hosted runners to:

* Increase security
* Access internal systems
* Improve performance

Our workshop uses the GitHub Hosted Runner.

---

# What are GitHub Secrets?

GitHub Secrets securely store sensitive information.

Examples include:

* AWS Access Keys
* SSH Private Keys
* API Tokens
* Database Passwords

Secrets are encrypted and cannot be viewed inside workflow logs.

Example

```yaml
${{ secrets.AWS_ACCESS_KEY_ID }}
```

GitHub replaces this placeholder with the actual value during execution.

---

# Repository Structure

Our repositories follow the following structure.

## Frontend Repository

```text
assignment-tracker-frontend

│

├── index.html

├── frontend_version.json

└── .github
    └── workflows
         └── frontend.yml
```

---

## Backend Repository

```text
assignment-tracker-backend

│

├── app.py

├── requirements.txt

├── backend_version.txt

└── .github
    └── workflows
         └── backend.yml
```

---

# Workflow Execution Lifecycle

The following diagram illustrates how GitHub executes a workflow.

```text
Developer

      │

      ▼

Git Push

      │

      ▼

GitHub Repository

      │

      ▼

Workflow File Detected

      │

      ▼

Runner Created

      │

      ▼

Repository Downloaded

      │

      ▼

Steps Executed

      │

      ▼

Workflow Finished

      │

      ▼

Runner Destroyed
```

Notice that the Runner is temporary.

Every workflow execution creates a brand-new virtual machine.

---

# YAML Best Practices

Always follow these guidelines when writing workflow files.

✅ Use spaces instead of tabs.

✅ Use meaningful job names.

✅ Keep workflows modular.

✅ Store credentials in GitHub Secrets.

✅ Avoid hardcoding sensitive information.

✅ Validate YAML indentation before committing.

---

# Common Beginner Mistakes

| Mistake               | Result                  |
| --------------------- | ----------------------- |
| Wrong indentation     | Workflow fails          |
| Wrong workflow folder | GitHub ignores workflow |
| Incorrect secret name | Authentication failure  |
| Hardcoded credentials | Security risk           |
| Wrong branch name     | Workflow never triggers |
| Typographical errors  | YAML parsing errors     |

  Section 4A.3:
  # Part 4A.3 – Frontend Deployment Workflow (`frontend.yml`)

---

# Introduction

In the previous sections, we learned:

* What GitHub Actions is.
* How GitHub Actions works.
* The concepts of Workflows, Jobs, Steps, Actions, and Runners.

Now, we will create our first GitHub Actions workflow that automatically deploys the frontend application to an Amazon S3 Static Website.

Instead of manually uploading HTML files after every modification, GitHub Actions will perform the deployment automatically whenever code is pushed to the **main** branch.

---

# Deployment Objective

Whenever a developer pushes code to the frontend repository, GitHub Actions should automatically:

1. Detect the code push.
2. Create a temporary Ubuntu Runner.
3. Download the latest repository.
4. Authenticate with AWS.
5. Upload the latest frontend files to Amazon S3.
6. Replace outdated files with the latest version.
7. Complete the deployment.

This entire process takes only a few seconds and requires no manual intervention.

---

# Frontend Deployment Architecture

```text
                   Developer
                        │
                        │ Git Push
                        ▼
             GitHub Frontend Repository
                        │
                        ▼
             GitHub Actions Workflow
                        │
         ┌──────────────┴──────────────┐
         │                             │
         ▼                             ▼
 Download Repository           Configure AWS
         │
         ▼
Upload Latest Files to Amazon S3
         │
         ▼
 Static Website Updated
```

---

# Where Should the Workflow File Be Created?

Open your frontend repository.

Create the following directory if it does not already exist.

```
.github/
└── workflows/
```

Inside the **workflows** folder, create a new file named:

```
frontend.yml
```

The final project structure should look like this:

```
assignment-tracker-frontend
│
├── index.html
├── frontend_version.json
└── .github
    └── workflows
        └── frontend.yml
```

GitHub automatically detects workflow files placed inside this directory.

---

# Complete Frontend Workflow

```yaml
name: Frontend Deployment

on:
  push:
    branches:
      - main

jobs:
  deploy-frontend:

    runs-on: ubuntu-latest

    steps:

      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ secrets.AWS_REGION }}

      - name: Deploy Frontend to Amazon S3
        run: |
          aws s3 sync . s3://${{ secrets.S3_BUCKET_NAME }} \
          --delete \
          --exclude ".git/*" \
          --exclude ".github/*"
```

---

# Understanding the Workflow Line by Line

Every statement in the workflow has a specific purpose. Let us understand each one.

---

## Line 1

```yaml
name: Frontend Deployment
```

### Purpose

This assigns a name to the workflow.

Whenever the workflow executes, GitHub displays this name on the **Actions** page.

Example:

```
Frontend Deployment
✔ Completed successfully
```

You may choose any meaningful name.

---

## Trigger Section

```yaml
on:
```

### Purpose

The **on** keyword specifies the event that starts the workflow.

Without a trigger, GitHub will never execute the workflow.

---

### Push Event

```yaml
push:
```

This means the workflow starts whenever new code is pushed.

GitHub supports many events such as:

* push
* pull_request
* release
* workflow_dispatch
* schedule

In this project, we only require **push**.

---

### Branch Selection

```yaml
branches:
  - main
```

This restricts execution to the **main** branch.

Example:

| Branch        | Workflow Executes? |
| ------------- | ------------------ |
| main          | ✔ Yes              |
| feature/login | ✘ No               |
| testing       | ✘ No               |

---

# Jobs Section

```yaml
jobs:
```

A workflow may contain one or more jobs.

Our workflow contains a single job.

```yaml
deploy-frontend:
```

The job name can be any valid identifier.

---

# Runner

```yaml
runs-on: ubuntu-latest
```

This instructs GitHub to create a temporary Ubuntu virtual machine.

Every execution receives a fresh environment.

GitHub automatically:

* Creates the VM.
* Downloads the repository.
* Executes all workflow steps.
* Deletes the VM after completion.

No manual cleanup is required.

---

# Steps

Every job consists of multiple steps.

Steps execute sequentially from top to bottom.

---

## Step 1 – Checkout Repository

```yaml
- name: Checkout Repository
```

This is a descriptive label.

It appears in the GitHub Actions execution log.

Example:

```
✔ Checkout Repository
```

---

### Action Used

```yaml
uses: actions/checkout@v4
```

This is an official GitHub Action.

Purpose:

* Connect to GitHub.
* Download the latest repository.
* Make repository files available inside the Runner.

Without this step, the Runner would not have access to your project files.

---

### Internal Execution

```
GitHub Repository
        │
        ▼
Download Repository
        │
        ▼
Ubuntu Runner
```

---

## Step 2 – Configure AWS Credentials

```yaml
- name: Configure AWS Credentials
```

This step authenticates the Runner with your AWS account.

---

### AWS Authentication Action

```yaml
uses: aws-actions/configure-aws-credentials@v4
```

This official GitHub Action performs:

* Secure login to AWS
* Temporary credential configuration
* Region selection

No AWS CLI login command is required.

---

### AWS Access Key

```yaml
aws-access-key-id:
```

Value:

```yaml
${{ secrets.AWS_ACCESS_KEY_ID }}
```

GitHub retrieves the value from Repository Secrets.

Example:

```
AKIA**************
```

The actual value is never displayed in logs.

---

### Secret Access Key

```yaml
aws-secret-access-key:
```

Value:

```yaml
${{ secrets.AWS_SECRET_ACCESS_KEY }}
```

Again, GitHub reads this securely from Repository Secrets.

---

### AWS Region

```yaml
aws-region:
```

Example:

```
us-east-1
```

This tells GitHub where your S3 bucket is located.

---

## Step 3 – Deploy Frontend

```yaml
run:
```

The **run** keyword executes Linux shell commands.

In this workflow, we execute one AWS CLI command.

---

### Upload Command

```bash
aws s3 sync . s3://${{ secrets.S3_BUCKET_NAME }}
```

This command synchronizes the local repository with the Amazon S3 bucket.

Unlike `aws s3 cp`, the **sync** command uploads only modified files, making deployments faster.

---

### Current Directory

```
.
```

The dot (`.`) represents the current working directory, which is the cloned repository.

---

### Destination Bucket

```
s3://assignment-tracker-frontend
```

The bucket name is obtained from:

```
${{ secrets.S3_BUCKET_NAME }}
```

---

### Delete Removed Files

```bash
--delete
```

Suppose you delete a file from the GitHub repository.

Without `--delete`, the old file would remain in the S3 bucket.

This option ensures that the bucket mirrors the repository exactly.

---

### Ignore Git Directory

```bash
--exclude ".git/*"
```

The `.git` folder contains version control metadata.

These files are not required on the website.

---

### Ignore Workflow Files

```bash
--exclude ".github/*"
```

Workflow files are only used by GitHub Actions.

They should never be uploaded to Amazon S3.

---

# Workflow Execution Sequence

Whenever code is pushed, GitHub performs the following sequence.

```text
Developer

      │

      ▼

Git Push

      │

      ▼

GitHub Repository

      │

      ▼

Workflow Triggered

      │

      ▼

Create Ubuntu Runner

      │

      ▼

Checkout Repository

      │

      ▼

Configure AWS Credentials

      │

      ▼

Upload Files to Amazon S3

      │

      ▼

Workflow Completed

      │

      ▼

Runner Destroyed
```

---

# Expected GitHub Actions Log

After a successful deployment, the **Actions** tab should display:

```
✔ Frontend Deployment

✔ Checkout Repository

✔ Configure AWS Credentials

✔ Deploy Frontend to Amazon S3

✔ Job completed successfully
```

---

# Verify the Deployment

Open your S3 Static Website URL.

Example:

```
http://assignment-tracker-frontend.s3-website-us-east-1.amazonaws.com
```

If the deployment was successful:

* The latest frontend version is displayed.
* Updated HTML changes are visible.
* Static assets load correctly.

---

# Common Errors

| Error                   | Cause                        | Solution                |
| ----------------------- | ---------------------------- | ----------------------- |
| Access Denied           | Invalid IAM permissions      | Verify IAM policy       |
| Invalid AWS Credentials | Incorrect Repository Secrets | Update secrets          |
| Bucket Not Found        | Incorrect bucket name        | Verify `S3_BUCKET_NAME` |
| Workflow Not Triggered  | Wrong branch                 | Push to `main`          |
| YAML Error              | Incorrect indentation        | Validate YAML syntax    |
| AWS Region Error        | Wrong region                 | Update `AWS_REGION`     |

# Imp Points 

* Use Repository Secrets for all credentials.
* Never hardcode AWS keys.
* Keep workflows inside `.github/workflows`.
* Use meaningful workflow and job names.
* Verify YAML indentation before committing.
* Restrict workflows to required branches.
# Part 4B.1A – Backend Deployment Architecture & Workflow File

---

# Introduction

In **Part 4A**, we successfully automated the deployment of our frontend application to Amazon S3 using GitHub Actions.

Whenever the developer pushed code to the GitHub repository, GitHub Actions automatically uploaded the latest frontend files to the S3 bucket, making them immediately available through the S3 Static Website.

Deploying the backend application is slightly different.

Unlike a static website, the backend is a **Flask application** that runs continuously on an Ubuntu EC2 instance. Simply copying the source code is not sufficient.

The deployment process must also:

* Connect securely to the EC2 instance.
* Download the latest application code.
* Install required Python libraries.
* Restart the backend server.
* Make the latest API available to the frontend.

GitHub Actions performs all these tasks automatically.

---

# Backend Deployment Objective

Whenever a developer pushes code to the **main** branch of the backend repository, GitHub Actions should automatically:

1. Detect the Git Push event.
2. Create a temporary Ubuntu Runner.
3. Download the backend repository.
4. Authenticate with the EC2 instance using SSH.
5. Connect to the Backend EC2 server.
6. Navigate to the deployment directory.
7. Clone or update the application source code.
8. Create (or reuse) the Python Virtual Environment.
9. Install the latest Python dependencies.
10. Restart the Flask application using Gunicorn.
11. Complete the deployment successfully.

The entire deployment process should occur without requiring manual login to the EC2 instance.

---

# Backend Deployment Architecture

The backend deployment architecture used in this workshop is shown below.

```text
                  Developer
                       │
                  Modify Code
                       │
                       ▼
                  Git Commit
                       │
                       ▼
                   Git Push
                       │
                       ▼
             GitHub Repository
                       │
               Push Event Detected
                       │
                       ▼
             GitHub Actions Runner
                       │
              Checkout Repository
                       │
                       ▼
           SSH Authentication
                       │
                       ▼
             Backend EC2 Server
                       │
        ┌──────────────┼──────────────┐
        │              │              │
        ▼              ▼              ▼
   Git Pull      Install Packages  Restart Gunicorn
        │              │              │
        └──────────────┴──────────────┘
                       │
                       ▼
               Flask REST API
                       │
                       ▼
               Amazon DynamoDB
```

---

# Why is Backend Deployment Different?

The frontend is a collection of static files.

Examples include:

* HTML
* CSS
* JavaScript
* Images

These files can simply be copied into an Amazon S3 bucket.

The backend, however, is an executable application.

It requires:

* Python Interpreter
* Flask Framework
* Gunicorn Web Server
* Python Libraries
* Environment Variables
* Running Processes

Therefore, backend deployment involves updating both the source code and the running application.

---

# Why Use SSH for Backend Deployment?

The backend application runs inside an Ubuntu EC2 instance.

GitHub Actions cannot directly modify files on the EC2 server.

Instead, it establishes a secure SSH connection and executes Linux commands remotely.

This approach provides several advantages:

* Secure encrypted communication
* Remote command execution
* Automated deployment
* No manual login required
* Widely used in production environments

---

# Backend Deployment Lifecycle

Every deployment follows the sequence below.

```text
Git Push
      │
      ▼
GitHub Actions Triggered
      │
      ▼
Create Ubuntu Runner
      │
      ▼
Download Repository
      │
      ▼
Authenticate using SSH
      │
      ▼
Connect to EC2
      │
      ▼
Execute Deployment Commands
      │
      ▼
Restart Backend
      │
      ▼
Deployment Completed
```

---

# Repository Structure

Your backend repository should now have the following structure.

```text
assignment-tracker-backend
│
├── app.py
├── requirements.txt
├── backend_version.txt
└── .github
     └── workflows
            └── backend.yml
```

Notice that the workflow file is stored inside:

```text
.github/workflows/
```

GitHub automatically detects workflow files located inside this directory.

---

# Creating the Workflow File

Inside the backend repository, create the following folder if it does not already exist.

```text
.github/
└── workflows/
```

Inside this folder, create a file named:

```text
backend.yml
```

The final directory structure should look like this.

```text
assignment-tracker-backend
│
├── app.py
├── backend_version.txt
├── requirements.txt
└── .github
     └── workflows
          └── backend.yml
```

---

# Complete Backend Workflow

Create the following workflow.

```yaml
name: Backend Deployment

on:
  push:
    branches:
      - main

jobs:
  deploy-backend:

    runs-on: ubuntu-latest

    steps:

      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Deploy Backend to EC2
        uses: appleboy/ssh-action@v1.0.3

        with:
          host: ${{ secrets.EC2_HOST }}
          username: ${{ secrets.EC2_USER }}
          key: ${{ secrets.EC2_SSH_KEY }}

          script: |

            mkdir -p ~/app

            if [ ! -d ~/app/.git ]; then
              git clone https://github.com/YOUR_GITHUB_USERNAME/assignment-tracker-backend.git ~/app
            fi

            cd ~/app

            git pull origin main

            python3 -m venv venv

            source venv/bin/activate

            pip install -r requirements.txt

            pkill gunicorn || true

            nohup gunicorn \
              -b 0.0.0.0:5000 \
              app:app \
              > backend.log 2>&1 &
```

> **Important**
>
> Replace:
>
> ```text
> YOUR_GITHUB_USERNAME
> ```
>
> with your own GitHub username before committing the workflow.

---

# High-Level Workflow Explanation

The workflow performs the following operations.

| Step                        | Description                                                   |
| --------------------------- | ------------------------------------------------------------- |
| Checkout Repository         | Downloads the latest backend source code to the GitHub Runner |
| SSH Authentication          | Connects securely to the EC2 instance                         |
| Create Deployment Directory | Creates `~/app` if it does not already exist                  |
| Clone Repository            | Downloads the repository during the first deployment          |
| Pull Latest Changes         | Updates the repository on subsequent deployments              |
| Create Virtual Environment  | Creates an isolated Python environment                        |
| Activate Environment        | Uses the virtual environment for package installation         |
| Install Dependencies        | Installs packages listed in `requirements.txt`                |
| Stop Previous Gunicorn      | Terminates the running backend process                        |
| Start New Gunicorn          | Launches the latest version of the Flask application          |

---

# How the Backend Workflow Executes

The execution sequence is illustrated below.

```text
Git Push
      │
      ▼
GitHub Repository
      │
      ▼
Backend Workflow
      │
      ▼
Checkout Repository
      │
      ▼
SSH Authentication
      │
      ▼
Ubuntu EC2
      │
      ▼
Git Pull
      │
      ▼
Create Virtual Environment
      │
      ▼
Install Dependencies
      │
      ▼
Restart Gunicorn
      │
      ▼
Backend Ready
```

---

# Why Do We Clone Only Once?

Notice the following block inside the workflow.

```bash
if [ ! -d ~/app/.git ]; then
    git clone ...
fi
```

This logic checks whether the backend repository has already been cloned.

### First Deployment

```text
Repository Not Found
        │
        ▼
Git Clone
```

### Subsequent Deployments

```text
Repository Already Exists
        │
        ▼
Git Pull
```

This approach significantly reduces deployment time because the repository is cloned only once.

---

# Backend Workflow Summary

The backend workflow combines several technologies into a single automated deployment pipeline.

| Technology          | Purpose                  |
| ------------------- | ------------------------ |
| GitHub Actions      | CI/CD Automation         |
| Ubuntu Runner       | Executes the workflow    |
| SSH                 | Secure remote connection |
| Git                 | Source code update       |
| Python              | Application runtime      |
| Virtual Environment | Dependency isolation     |
| Gunicorn            | Flask production server  |
| Amazon EC2          | Backend hosting          |
| Amazon DynamoDB     | Data storage             |

# Part 4B.1B – Line-by-Line Explanation of `backend.yml`

---

# Introduction

In **Part 4B.1A**, we created the complete backend deployment workflow.

However, simply copying the workflow is not enough.

A DevOps Engineer should understand:

* Why each keyword is used.
* What GitHub Actions does internally.
* How SSH authentication works.
* How the EC2 deployment is performed.
* How the Flask application is restarted.

This section explains every part of the workflow in detail.

---

# Complete Backend Workflow

```yaml
name: Backend Deployment

on:
  push:
    branches:
      - main

jobs:
  deploy-backend:

    runs-on: ubuntu-latest

    steps:

      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Deploy Backend to EC2
        uses: appleboy/ssh-action@v1.0.3

        with:
          host: ${{ secrets.EC2_HOST }}
          username: ${{ secrets.EC2_USER }}
          key: ${{ secrets.EC2_SSH_KEY }}

          script: |

            mkdir -p ~/app

            if [ ! -d ~/app/.git ]; then
              git clone https://github.com/YOUR_GITHUB_USERNAME/assignment-tracker-backend.git ~/app
            fi

            cd ~/app

            git pull origin main

            python3 -m venv venv

            source venv/bin/activate

            pip install -r requirements.txt

            pkill gunicorn || true

            nohup gunicorn \
              -b 0.0.0.0:5000 \
              app:app \
              > backend.log 2>&1 &
```

---

# Workflow Name

```yaml
name: Backend Deployment
```

## Purpose

This gives a human-readable name to the workflow.

It appears in the GitHub **Actions** tab.

Example:

```
Backend Deployment
✔ Completed Successfully
```

You may choose any meaningful name.

---

# Trigger Section

```yaml
on:
```

## Purpose

The `on` keyword tells GitHub **when** the workflow should start.

Without a trigger, the workflow will never execute.

---

# Push Event

```yaml
push:
```

This specifies that the workflow should execute whenever code is pushed to the repository.

GitHub supports many events, such as:

* `push`
* `pull_request`
* `release`
* `workflow_dispatch`
* `schedule`

For this project, we use the **push** event.

---

# Branch Filter

```yaml
branches:
  - main
```

This restricts the workflow to the `main` branch.

| Branch        | Workflow Executes? |
| ------------- | ------------------ |
| main          | ✅ Yes              |
| feature/login | ❌ No               |
| testing       | ❌ No               |

This prevents deployments from incomplete feature branches.

---

# Jobs Section

```yaml
jobs:
```

A workflow can contain one or more **Jobs**.

Examples:

* Build
* Test
* Deploy

Our workflow contains one job:

```yaml
deploy-backend:
```

This is simply the job identifier.

GitHub displays it in the execution log.

---

# Runner

```yaml
runs-on: ubuntu-latest
```

This tells GitHub to create a temporary Ubuntu virtual machine.

Internally:

```
GitHub

↓

Create Ubuntu VM

↓

Download Repository

↓

Execute Workflow

↓

Delete VM
```

Every execution starts with a clean environment.

---

# Steps Section

```yaml
steps:
```

A Job is divided into multiple **Steps**.

Each step performs one task.

GitHub executes them sequentially.

---

# Step 1 – Checkout Repository

```yaml
- name: Checkout Repository
```

This is a descriptive label shown in the Actions log.

Example:

```
✔ Checkout Repository
```

---

# GitHub Action Used

```yaml
uses: actions/checkout@v4
```

This is an official GitHub Action.

## What does it do?

Without this action, the Runner is empty.

It automatically:

* Connects to GitHub
* Downloads the repository
* Checks out the latest commit
* Makes the source code available

Internally:

```
GitHub Repository

↓

Download Source Code

↓

Ubuntu Runner
```

---

# Step 2 – Deploy Backend

```yaml
- name: Deploy Backend to EC2
```

This is another descriptive name displayed in the workflow log.

---

# SSH Action

```yaml
uses: appleboy/ssh-action@v1.0.3
```

This is one of the most widely used GitHub Actions for remote deployments.

Instead of writing custom SSH scripts, this Action:

* Creates the SSH connection
* Authenticates using the private key
* Executes remote commands
* Returns the execution output to GitHub

---

# Why Do We Need SSH?

The backend application runs on a remote EC2 instance.

GitHub Actions cannot directly access files on EC2.

Instead, it performs the following sequence:

```
GitHub Runner

↓

SSH Connection

↓

Ubuntu EC2

↓

Execute Linux Commands
```

SSH provides:

* Encrypted communication
* Secure authentication
* Remote command execution

---

# with:

```yaml
with:
```

The `with` block passes configuration values to the `appleboy/ssh-action`.

Think of it as providing input parameters to the Action.

---

# Host

```yaml
host: ${{ secrets.EC2_HOST }}
```

This specifies the destination server.

Example:

```
54.210.xxx.xxx
```

Rather than hardcoding the IP address, we store it as a Repository Secret.

Benefits:

* Improved security
* Easy updates
* Reusable workflows

---

# Username

```yaml
username: ${{ secrets.EC2_USER }}
```

Specifies the Linux user account.

For Ubuntu EC2 instances:

```
ubuntu
```

Amazon Linux instances would use:

```
ec2-user
```

---

# SSH Private Key

```yaml
key: ${{ secrets.EC2_SSH_KEY }}
```

This is the most critical line of the workflow.

GitHub retrieves the private key from Repository Secrets.

Internally:

```
Repository Secret

↓

Temporary SSH Key

↓

Authenticate to EC2
```

Never commit the private key to GitHub.

---

# Script Block

```yaml
script: |
```

Everything below this line consists of Linux shell commands executed **on the EC2 instance**, not on the GitHub Runner.

This distinction is important.

### Runs on GitHub Runner

```yaml
actions/checkout
```

### Runs on EC2

```bash
mkdir

git pull

pip install

gunicorn
```

---

# Understanding the Script Block

The script executes the following operations:

```
SSH Login

↓

Create Directory

↓

Clone Repository (First Deployment)

↓

Git Pull

↓

Create Virtual Environment

↓

Install Packages

↓

Stop Gunicorn

↓

Start Gunicorn
```

Each command is executed sequentially.

If a command fails (unless explicitly ignored), the workflow stops and is marked as failed.

---

# Internal Workflow Execution

The complete execution sequence is shown below.

```
Git Push

↓

GitHub Repository

↓

Workflow Triggered

↓

Ubuntu Runner Created

↓

Checkout Repository

↓

Read Repository Secrets

↓

SSH Authentication

↓

Connect to EC2

↓

Execute Deployment Script

↓

Deployment Completed

↓

Runner Destroyed
```

---

# Why Store Credentials in GitHub Secrets?

Hardcoding credentials is one of the most common security mistakes.

❌ Incorrect

```yaml
host: 54.210.xx.xx

username: ubuntu

key: -----BEGIN PRIVATE KEY-----
```

✅ Correct

```yaml
host: ${{ secrets.EC2_HOST }}

username: ${{ secrets.EC2_USER }}

key: ${{ secrets.EC2_SSH_KEY }}
```

Advantages:

* Encrypted storage
* Hidden in logs
* Easy to update
* No source code exposure

---

# Common Beginner Mistakes

| Mistake               | Result                  |
| --------------------- | ----------------------- |
| Incorrect secret name | Authentication fails    |
| Wrong EC2 IP          | SSH timeout             |
| Invalid private key   | Permission denied       |
| Missing checkout step | Repository unavailable  |
| Incorrect indentation | YAML parsing error      |
| Wrong branch          | Workflow never triggers |

---

# Best Practices

* Use meaningful workflow names.
* Keep one responsibility per job.
* Store credentials only in GitHub Secrets.
* Never commit private keys.
* Validate YAML indentation before pushing.
* Keep workflow files inside `.github/workflows`.
* Use descriptive step names.
# Part 4B.2 – Deployment Commands, Verification & Best Practices

---

# Introduction

In **Part 4B.1**, we created the `backend.yml` workflow and understood the overall deployment architecture.

However, understanding a workflow is not enough.

To become confident with GitHub Actions, you should also understand **every deployment command** executed on the Backend EC2 server.

This chapter explains each command in detail, including:

* Why the command is required
* What happens internally
* Expected output
* Common mistakes
* Best practices

By the end of this section, you will have a complete understanding of how GitHub Actions deploys the Flask application.

---

# Deployment Commands Overview

When GitHub Actions successfully establishes an SSH connection with the Backend EC2 instance, it executes the following sequence of Linux commands.

```text
Connect to EC2
      │
      ▼
Create Deployment Directory
      │
      ▼
Clone Repository (First Time)
      │
      ▼
Pull Latest Changes
      │
      ▼
Create Virtual Environment
      │
      ▼
Activate Virtual Environment
      │
      ▼
Install Dependencies
      │
      ▼
Stop Previous Gunicorn
      │
      ▼
Start New Gunicorn
      │
      ▼
Backend Deployment Completed
```

---

# Command 1: Create the Deployment Directory

```bash
mkdir -p ~/app
```

## Purpose

Creates the deployment directory where the backend application will be stored.

The `-p` option ensures that:

* The directory is created if it does not exist.
* No error is generated if the directory already exists.

### First Deployment

```text
Home Directory
│
└── app/
```

### Subsequent Deployments

The command executes safely without modifying the existing directory.

---

# Command 2: Clone the Repository

```bash
if [ ! -d ~/app/.git ]; then
    git clone https://github.com/YOUR_GITHUB_USERNAME/assignment-tracker-backend.git ~/app
fi
```

## Why is this required?

The repository must exist on the EC2 server.

During the **first deployment**, the repository does not exist, so GitHub Actions clones it.

### Internal Logic

```text
Repository Exists?
      │
 ┌────┴────┐
 │         │
No        Yes
 │         │
 ▼         ▼
Clone    Skip
```

### Expected Output

```text
Cloning into '~/app'...
Receiving objects...
Resolving deltas...
```

---

# Command 3: Navigate to the Repository

```bash
cd ~/app
```

## Purpose

Changes the current working directory.

Every subsequent command executes inside the application directory.

Without this command, Git would attempt to update the wrong directory.

---

# Command 4: Pull the Latest Source Code

```bash
git pull origin main
```

## Purpose

Downloads the latest source code from GitHub.

### Internal Flow

```text
GitHub Repository
        │
        ▼
Compare Commits
        │
        ▼
Download Changes
        │
        ▼
Update Local Repository
```

### Expected Output

```text
Updating...
Fast-forward
```

or

```text
Already up to date.
```

---

# Why Use `git pull` Instead of `git clone` Every Time?

`git clone` downloads the entire repository.

`git pull` downloads only the changes.

Benefits include:

* Faster deployments
* Lower bandwidth usage
* Smaller execution time

---

# Command 5: Create the Python Virtual Environment

```bash
python3 -m venv venv
```

## Purpose

Creates an isolated Python environment.

Directory structure:

```text
app
│
├── venv/
│
├── app.py
│
├── requirements.txt
│
└── backend_version.txt
```

### Why is this important?

Without a virtual environment:

* Packages are installed globally.
* Different projects may conflict.
* Version mismatches become common.

---

# Command 6: Activate the Virtual Environment

```bash
source venv/bin/activate
```

## Purpose

Switches the current shell to the virtual environment.

Expected terminal prompt:

```text
(venv) ubuntu@ip-172-31-xx-xx:~/app$
```

All Python packages installed after activation remain inside the virtual environment.

---

# Command 7: Install Dependencies

```bash
pip install -r requirements.txt
```

## Purpose

Installs all Python packages required by the Flask application.

Typical packages include:

* Flask
* Gunicorn
* Boto3
* Requests

### Internal Flow

```text
requirements.txt
        │
        ▼
Read Package List
        │
        ▼
Download Packages
        │
        ▼
Install Packages
```

### Expected Output

```text
Successfully installed Flask
Successfully installed Gunicorn
Successfully installed boto3
```

---

# Why Install Packages During Every Deployment?

Suppose a developer adds a new dependency.

Example:

```text
Flask
↓

Flask
Pandas
```

The workflow automatically installs the new package.

No manual package installation is required.

---

# Command 8: Stop the Existing Gunicorn Process

```bash
pkill gunicorn || true
```

## Purpose

Stops the previously running backend process.

Without this step:

* Multiple Gunicorn instances may run.
* Port conflicts may occur.
* The new version cannot start.

### Why `|| true`?

If Gunicorn is not running, `pkill` returns an error.

Normally, this would stop the workflow.

Using `|| true` tells GitHub Actions to ignore this specific error and continue execution.

---

# Command 9: Start the Flask Application

```bash
nohup gunicorn \
-b 0.0.0.0:5000 \
app:app \
> backend.log 2>&1 &
```

This is the most important command in the workflow.

---

## Understanding the Command

### `nohup`

Runs the application in the background.

The application continues running even after the SSH session ends.

---

### `gunicorn`

Production-grade WSGI server for Flask.

Much faster and more reliable than Flask's built-in development server.

---

### `-b`

Specifies the network binding.

```text
0.0.0.0
```

Allows connections from any IP address.

---

### `5000`

Application port.

Matches the Security Group rule configured earlier.

---

### `app:app`

This has two parts.

```
app.py
```

↓

```
app
```

The second `app` refers to the Flask application object.

Example:

```python
app = Flask(__name__)
```

---

### `> backend.log`

Redirects all output to:

```text
backend.log
```

instead of printing it on the terminal.

---

### `2>&1`

Redirects both:

* Standard Output
* Standard Error

to the same log file.

---

### `&`

Runs the application in the background.

Without `&`, GitHub Actions would wait forever and eventually fail due to timeout.

---

# Deployment Flow Diagram

```text
GitHub Runner
      │
      ▼
SSH Login
      │
      ▼
mkdir ~/app
      │
      ▼
git pull
      │
      ▼
Create Virtual Environment
      │
      ▼
Install Packages
      │
      ▼
Stop Gunicorn
      │
      ▼
Start Gunicorn
      │
      ▼
Deployment Successful
```

---

# Verifying the Deployment

After the workflow completes successfully, verify the deployment.

---

## Check Running Gunicorn Process

```bash
ps -ef | grep gunicorn
```

Expected output:

```text
ubuntu   12345   1   0  gunicorn
```

---

## Verify Backend API

Open:

```text
http://<EC2-PUBLIC-IP>:5000
```

or

```text
http://<EC2-PUBLIC-IP>:5000/version
```

The application should respond successfully.

---

## View Backend Log

```bash
cat ~/app/backend.log
```

---

## Monitor Live Logs

```bash
tail -f ~/app/backend.log
```

Press:

```
Ctrl + C
```

to stop monitoring.

---

# GitHub Actions Execution Log

A successful workflow should display something similar to:

```text
✔ Checkout Repository

✔ Deploy Backend to EC2

✔ Git Pull

✔ Install Dependencies

✔ Restart Gunicorn

✔ Deployment Completed
```

---

# Common Deployment Errors

| Error                         | Possible Cause           | Solution                           |
| ----------------------------- | ------------------------ | ---------------------------------- |
| Permission denied (publickey) | Incorrect SSH key        | Verify `EC2_SSH_KEY`               |
| Host unreachable              | Wrong EC2 IP             | Verify `EC2_HOST`                  |
| Repository not found          | Incorrect GitHub URL     | Update repository URL              |
| ModuleNotFoundError           | Missing package          | Update `requirements.txt`          |
| gunicorn: command not found   | Gunicorn not installed   | Add Gunicorn to `requirements.txt` |
| Address already in use        | Existing process running | Verify `pkill gunicorn`            |
| 502/Connection Refused        | Gunicorn failed          | Check `backend.log`                |

---

# Best Practices

Always follow these recommendations.

### Use Virtual Environments

Never install packages globally.

---

### Store Credentials Securely

Use GitHub Secrets instead of hardcoding credentials.

---

### Keep `requirements.txt` Updated

Every new dependency should be committed to GitHub.

---

### Monitor Application Logs

Regularly inspect:

```text
backend.log
```

for deployment issues.

---

### Use Meaningful Commit Messages

Example:

```text
Added Assignment API

Updated Submission Module

Fixed Authentication Bug
```

Avoid vague messages such as:

```text
update

changes

fixed
```

---

### Test Locally Before Pushing

Verify the application works correctly before triggering the CI/CD pipeline.

---

# Deployment Checklist

Before considering the backend deployment complete, verify:

| Task                        | Status |
| --------------------------- | ------ |
| Workflow Executed           | ✅      |
| SSH Connection Successful   | ✅      |
| Repository Updated          | ✅      |
| Virtual Environment Created | ✅      |
| Dependencies Installed      | ✅      |
| Gunicorn Restarted          | ✅      |
| Backend API Accessible      | ✅      |
| Logs Verified               | ✅      |

# Part 4C – End-to-End Deployment, Monitoring, Verification, and Complete CI/CD Pipeline

---

# Introduction

Congratulations! 🎉

At this stage, you have successfully configured:

* AWS Infrastructure
* Amazon S3 Static Website
* Backend EC2 Instance
* Amazon DynamoDB
* IAM Roles
* GitHub Repositories
* GitHub Secrets
* Frontend Workflow (`frontend.yml`)
* Backend Workflow (`backend.yml`)

The project is now capable of automatically deploying both the frontend and backend applications whenever code is pushed to GitHub.

In this section, we will understand the complete deployment lifecycle, monitor workflow execution, verify successful deployment, and troubleshoot common issues.

---

# End-to-End CI/CD Pipeline

The complete deployment process is illustrated below.

```text
                      Developer
                           │
                    Modify Source Code
                           │
                           ▼
                     Git Commit
                           │
                           ▼
                      Git Push
                           │
                           ▼
                 GitHub Repository
                           │
             Push Event Detected
                           │
                           ▼
                GitHub Actions Runner
                 ┌─────────────────────┐
                 │                     │
                 ▼                     ▼
        Frontend Workflow      Backend Workflow
                 │                     │
                 ▼                     ▼
        Upload Files to S3      SSH into EC2
                                      │
                                      ▼
                             Pull Latest Code
                                      │
                                      ▼
                            Install Dependencies
                                      │
                                      ▼
                             Restart Gunicorn
                                      │
                                      ▼
                               Flask Application
                                      │
                                      ▼
                               Amazon DynamoDB
```

---

# Step 1: First Deployment

Once all files have been added to the repositories, perform the initial deployment.

## Frontend Repository

```bash
git add .

git commit -m "Initial frontend deployment"

git push origin main
```

---

## Backend Repository

```bash
git add .

git commit -m "Initial backend deployment"

git push origin main
```

The moment you execute the `git push` command, GitHub automatically starts the corresponding workflow.

No manual deployment is required.

---

# What Happens After Git Push?

Many beginners believe that GitHub simply stores the code after a push.

In reality, GitHub performs several automated tasks.

The following sequence occurs after every push.

```text
Developer
      │
      ▼
Git Push
      │
      ▼
GitHub receives commit
      │
      ▼
Checks for workflow files
      │
      ▼
Creates Ubuntu Runner
      │
      ▼
Downloads repository
      │
      ▼
Reads Repository Secrets
      │
      ▼
Executes Workflow
      │
      ▼
Deploys Application
      │
      ▼
Destroys Runner
```

Every workflow execution receives a brand-new virtual machine.

---

# Frontend Deployment Sequence

```text
Git Push
      │
      ▼
GitHub Actions
      │
      ▼
Checkout Repository
      │
      ▼
Authenticate with AWS
      │
      ▼
Synchronize Files
      │
      ▼
Amazon S3
      │
      ▼
Static Website Updated
```

---

# Backend Deployment Sequence

```text
Git Push
      │
      ▼
GitHub Actions
      │
      ▼
SSH into EC2
      │
      ▼
Navigate to ~/app
      │
      ▼
Git Pull
      │
      ▼
Activate Virtual Environment
      │
      ▼
Install Dependencies
      │
      ▼
Stop Existing Gunicorn
      │
      ▼
Start New Gunicorn
      │
      ▼
Backend Updated
```

---

# Monitoring GitHub Actions

GitHub provides a dedicated **Actions** tab where every workflow execution can be monitored.

## Open Workflow Dashboard

1. Open the repository.
2. Click **Actions**.

You will see a list of all workflow executions.

---

# Understanding Workflow Status

GitHub displays different icons indicating the execution status.

| Icon | Meaning                         |
| ---- | ------------------------------- |
| 🟢   | Workflow Completed Successfully |
| 🟡   | Workflow Currently Running      |
| 🔴   | Workflow Failed                 |
| ⚪    | Workflow Waiting                |
| ⛔    | Workflow Cancelled              |

---

# Viewing Workflow Logs

Select a workflow execution.

GitHub displays:

```
Frontend Deployment

Checkout Repository

Configure AWS Credentials

Deploy Frontend
```

or

```
Backend Deployment

Checkout Repository

SSH Deployment

Install Requirements

Restart Gunicorn
```

Click any step to expand the detailed execution log.

---

# Re-running a Workflow

If a workflow fails:

1. Open the failed execution.
2. Click **Re-run Jobs**.
3. GitHub creates a new Runner.
4. The workflow executes again.

This avoids the need for another Git push when only a temporary issue caused the failure.

---

# Deployment Verification

After the workflows complete successfully, verify that the application is accessible.

---

## Verify Frontend

Open your Amazon S3 Static Website URL.

Example

```
http://assignment-tracker-frontend.s3-website-us-east-1.amazonaws.com
```

Verify:

* Website loads successfully.
* Latest UI changes are visible.
* Version number is updated.
* JavaScript functions correctly.
* Backend API requests are successful.

---

## Verify Backend

Open

```
http://<EC2-PUBLIC-IP>:5000
```

If your application includes a version endpoint, open:

```
http://<EC2-PUBLIC-IP>:5000/version
```

Verify:

* Flask server is running.
* Latest backend version is displayed.
* API endpoints respond successfully.

---

# Verify DynamoDB

Open the AWS Console.

Navigate to:

```
Amazon DynamoDB
```

Select:

```
ASSIGNMENTS
```

Verify:

* Assignment records are stored.

Next, open:

```
SUBMISSIONS
```

Verify:

* Student submissions are recorded correctly.

---

# Complete Application Testing

Perform the following end-to-end test.

| Test Case           | Expected Result      |
| ------------------- | -------------------- |
| Open Frontend       | Website Loads        |
| Create Assignment   | Record Stored        |
| Submit Assignment   | Record Stored        |
| Retrieve Assignment | Data Displayed       |
| Refresh Website     | Data Persists        |
| Update Code         | Automatic Deployment |

---

# Common Deployment Errors

## Frontend Errors

| Error               | Cause                 | Solution                       |
| ------------------- | --------------------- | ------------------------------ |
| AccessDenied        | IAM Policy Missing    | Attach `AmazonS3FullAccess`    |
| Bucket Not Found    | Incorrect Bucket Name | Verify `S3_BUCKET_NAME`        |
| Invalid Region      | Wrong AWS Region      | Update `AWS_REGION`            |
| Invalid Credentials | Incorrect Secrets     | Reconfigure Repository Secrets |

---

## Backend Errors

| Error               | Cause                | Solution                 |
| ------------------- | -------------------- | ------------------------ |
| Permission Denied   | Incorrect SSH Key    | Verify `EC2_SSH_KEY`     |
| Host Unreachable    | Wrong Public IP      | Update `EC2_HOST`        |
| Gunicorn Not Found  | Package Missing      | Install Gunicorn         |
| ModuleNotFoundError | Missing Dependencies | Check `requirements.txt` |
| Flask Not Running   | Gunicorn Failed      | Check `backend.log`      |

---

# Useful Debugging Commands

SSH into the Backend EC2 instance.

### Check Running Processes

```bash
ps -ef | grep gunicorn
```

---

### View Backend Log

```bash
cat ~/app/backend.log
```

---

### View Last 50 Lines

```bash
tail -50 ~/app/backend.log
```

---

### Verify Virtual Environment

```bash
ls ~/app/venv
```

---

### Check Python Version

```bash
python3 --version
```

---

### Verify Git Repository

```bash
cd ~/app

git status
```

---

# Best Practices

* Keep GitHub Secrets updated.
* Never store credentials in source code.
* Use meaningful commit messages.
* Test locally before pushing.
* Verify workflow status after every deployment.
* Monitor application logs regularly.
* Rotate AWS Access Keys periodically.
* Restrict IAM permissions to the minimum required.

---

# Complete CI/CD Architecture

```text
                           Developer
                                │
                         Modify Source Code
                                │
                                ▼
                          Git Commit
                                │
                                ▼
                           Git Push
                                │
                                ▼
                      GitHub Repository
                                │
                        Push Event Triggered
                                │
                                ▼
                     GitHub Actions Workflow
                 ┌───────────────────────────────┐
                 │                               │
                 ▼                               ▼
         Frontend Deployment             Backend Deployment
                 │                               │
         Checkout Repository             Checkout Repository
                 │                               │
        Configure AWS Credentials        SSH into EC2
                 │                               │
          Upload Files to S3             Pull Latest Code
                 │                               │
                 ▼                        Install Dependencies
         Amazon S3 Website                     │
                 │                             ▼
                 │                     Restart Gunicorn
                 │                             │
                 └──────────────┐              ▼
                                ▼
                         Flask REST API
                                │
                                ▼
                        Amazon DynamoDB
```

---

# Learning Outcomes

After completing this workshop, students should be able to:

* Understand the complete CI/CD lifecycle.
* Configure GitHub Actions workflows.
* Deploy static websites to Amazon S3.
* Deploy Flask applications to Amazon EC2.
* Secure deployments using SSH keys and GitHub Secrets.
* Monitor workflow execution.
* Debug deployment failures.
* Verify successful application deployment.
* Implement a production-style deployment pipeline.

---

# Final Checklist

Before concluding the workshop, verify that all tasks have been completed.

| Task                           | Status |
| ------------------------------ | ------ |
| AWS Infrastructure Created     | ✅      |
| Amazon S3 Configured           | ✅      |
| Backend EC2 Running            | ✅      |
| DynamoDB Tables Created        | ✅      |
| IAM Roles Configured           | ✅      |
| GitHub Repositories Created    | ✅      |
| GitHub Secrets Configured      | ✅      |
| Frontend Workflow Created      | ✅      |
| Backend Workflow Created       | ✅      |
| Frontend Deployment Successful | ✅      |
| Backend Deployment Successful  | ✅      |
| End-to-End Testing Completed   | ✅      |

🚀 Part 5:

# Part 5 – Testing, Validation, Troubleshooting & Project Completion

---

# Project Completion

Congratulations! 🎉

You have successfully built a complete **CI/CD Deployment Pipeline** using **GitHub Actions** and **Amazon Web Services (AWS)**.

Throughout this workshop, you have learned how to:

* Create AWS Infrastructure
* Configure Amazon S3 Static Website
* Launch and Configure Amazon EC2
* Create Amazon DynamoDB Tables
* Configure IAM Roles and IAM Users
* Secure Deployments using SSH Keys
* Configure GitHub Repository Secrets
* Create GitHub Actions Workflows
* Automatically Deploy Frontend Applications
* Automatically Deploy Backend Applications
* Monitor CI/CD Workflows
* Verify Successful Deployments

Before concluding the workshop, perform the following validation steps.

---

# Section 1 – Deployment Validation

A deployment should always be validated after every successful workflow execution.

Use the following checklist.

| Validation Item                | Expected Result        | Status |
| ------------------------------ | ---------------------- | ------ |
| GitHub Workflow Executed       | Completed Successfully | ☐      |
| Frontend Uploaded to Amazon S3 | Success                | ☐      |
| Backend Workflow Executed      | Success                | ☐      |
| SSH Login Successful           | Success                | ☐      |
| Latest Source Code Pulled      | Success                | ☐      |
| Python Dependencies Installed  | Success                | ☐      |
| Gunicorn Restarted             | Success                | ☐      |
| Backend API Accessible         | Success                | ☐      |
| DynamoDB Connected             | Success                | ☐      |
| Static Website Accessible      | Success                | ☐      |

---

# Section 2 – Functional Testing

After deployment, verify the application from an end-user perspective.

---

## Test Case 1 – Frontend Accessibility

### Objective

Verify that the frontend website is accessible.

### Steps

1. Open the S3 Static Website URL.
2. Verify that the webpage loads successfully.
3. Refresh the page.

### Expected Result

* Website loads successfully.
* No broken layout.
* Images load correctly.
* CSS loads correctly.
* JavaScript executes correctly.

---

## Test Case 2 – Backend API

### Objective

Verify that the Flask backend is running.

### Steps

Open

```text
http://<EC2-PUBLIC-IP>:5000
```

or

```text
http://<EC2-PUBLIC-IP>:5000/version
```

### Expected Result

API returns the expected response.

---

## Test Case 3 – Assignment Creation

### Objective

Verify that assignments are stored in DynamoDB.

### Steps

* Create a new assignment.
* Submit the request.

### Expected Result

A new record is inserted into the **ASSIGNMENTS** table.

---

## Test Case 4 – Student Submission

### Objective

Verify backend database operations.

### Steps

* Submit an assignment.

### Expected Result

A new record appears inside the **SUBMISSIONS** table.

---

## Test Case 5 – CI/CD Automation

### Objective

Verify automatic deployment.

### Steps

Modify

```text
frontend_version.json
```

Change

```json
{
    "version":"1.0"
}
```

to

```json
{
    "version":"1.1"
}
```

Commit

```bash
git add .

git commit -m "Updated frontend version"

git push origin main
```

### Expected Result

GitHub Actions automatically deploys the latest frontend.

---

# Section 3 – Workflow Validation

Open

```
GitHub Repository

↓

Actions
```

Verify:

| Workflow            | Status |
| ------------------- | ------ |
| Frontend Deployment | ✅      |
| Backend Deployment  | ✅      |

Each workflow should display:

```
Completed Successfully
```

---

# Section 4 – Backend Validation Commands

SSH into the EC2 instance.

---

## Verify Repository

```bash
cd ~/app

git status
```

Expected

```
On branch main

Your branch is up to date.
```

---

## Verify Gunicorn

```bash
ps -ef | grep gunicorn
```

Expected

```
gunicorn running
```

---

## Verify Python Environment

```bash
source ~/app/venv/bin/activate

python --version
```

---

## View Backend Logs

```bash
cat ~/app/backend.log
```

---

## Monitor Logs

```bash
tail -f ~/app/backend.log
```

---

# Section 5 – GitHub Actions Logs

GitHub provides detailed execution logs.

Navigate to

```
Repository

↓

Actions

↓

Workflow

↓

Job

↓

Step
```

Example

```
Checkout Repository

Configure AWS

SSH into EC2

Git Pull

Install Requirements

Restart Gunicorn
```

These logs help identify deployment failures.

---

# Section 6 – Troubleshooting Guide

The following table lists common deployment problems and their solutions.

| Problem                       | Possible Cause            | Solution                              |
| ----------------------------- | ------------------------- | ------------------------------------- |
| Workflow did not start        | Wrong branch              | Push to `main`                        |
| Workflow not visible          | Incorrect workflow folder | Place file inside `.github/workflows` |
| Permission denied (publickey) | Incorrect SSH key         | Update `EC2_SSH_KEY`                  |
| Authentication failed         | Incorrect AWS Credentials | Update GitHub Secrets                 |
| Bucket not found              | Incorrect bucket name     | Verify `S3_BUCKET_NAME`               |
| Gunicorn not found            | Missing package           | Install Gunicorn                      |
| ModuleNotFoundError           | Missing dependency        | Update `requirements.txt`             |
| Address already in use        | Existing Gunicorn process | Execute `pkill gunicorn`              |
| Website not updated           | Browser cache             | Refresh or clear cache                |
| DynamoDB access denied        | IAM Role missing          | Attach required IAM policy            |

---

# Section 7 – Common GitHub Actions Errors

---

## YAML Syntax Error

Example

```
Invalid workflow file
```

Solution

* Verify indentation.
* Use spaces instead of tabs.

---

## Secret Not Found

Example

```
Secret EC2_HOST not found
```

Solution

Create the repository secret.

---

## SSH Authentication Failure

Example

```
Permission denied (publickey)
```

Solution

Verify:

* EC2_SSH_KEY
* authorized_keys
* EC2_USER

---

## AWS Authentication Failure

Example

```
InvalidAccessKeyId
```

Solution

Generate a new AWS Access Key.

---

## Workflow Timeout

Possible causes

* Application waiting for input
* Infinite loop
* Gunicorn running in foreground

Solution

Use

```bash
nohup gunicorn &
```

---

# Section 8 – Security Best Practices

Always follow these recommendations.

### GitHub

* Never commit secrets.
* Never commit SSH keys.
* Protect the main branch.
* Enable Two-Factor Authentication.

---

### AWS

* Use IAM Roles.
* Rotate Access Keys regularly.
* Follow the Principle of Least Privilege.
* Never use the Root User for deployments.

---

### Linux

* Use SSH Keys instead of passwords.
* Keep Ubuntu updated.
* Remove unused packages.
* Monitor application logs.

---

# Section 9 – Future Improvements

The current project demonstrates a modern CI/CD pipeline.

However, production environments generally include additional services.

Possible enhancements include:

## Infrastructure

* Amazon CloudFront
* AWS WAF
* Route 53
* Elastic Load Balancer
* Auto Scaling Group

---

## DevOps

* Docker
* Docker Compose
* Kubernetes
* Amazon ECS
* Amazon EKS
* Terraform
* AWS CodeDeploy
* AWS CodePipeline

---

## Monitoring

* Amazon CloudWatch
* Grafana
* Prometheus
* ELK Stack

---

## Security

* AWS Secrets Manager
* AWS Systems Manager Parameter Store
* IAM Least Privilege Policies
* SSL/TLS Certificates

---

## Database

* Amazon RDS
* Aurora PostgreSQL
* Amazon ElastiCache
* Amazon OpenSearch

---

# Section 10 – Learning Outcomes

After completing this workshop, students should be able to:

* Understand DevOps principles.
* Configure GitHub repositories.
* Use Git effectively.
* Configure GitHub Actions.
* Deploy Static Websites to Amazon S3.
* Deploy Flask Applications to Amazon EC2.
* Configure IAM Users and Roles.
* Configure GitHub Repository Secrets.
* Secure applications using SSH Keys.
* Use Amazon DynamoDB.
* Understand CI/CD pipelines.
* Monitor workflow execution.
* Troubleshoot deployment failures.

---

# Section 11 – Skills Acquired

This workshop provides hands-on experience with:

| Category        | Skills                          |
| --------------- | ------------------------------- |
| Version Control | Git, GitHub                     |
| DevOps          | GitHub Actions                  |
| Cloud           | Amazon EC2, Amazon S3, DynamoDB |
| Linux           | Ubuntu Administration           |
| Security        | IAM, SSH Keys                   |
| Backend         | Flask, Gunicorn                 |
| Automation      | CI/CD Pipelines                 |

---

# Section 12 – References

Official Documentation

* GitHub Actions Documentation
* AWS EC2 Documentation
* AWS S3 Documentation
* AWS IAM Documentation
* AWS DynamoDB Documentation
* Flask Documentation
* Gunicorn Documentation
* Git Documentation

---

# Section 13 – Contributors

## Project Author

**Dr. AJ**

Assistant Professor

Department of Computer Engineering

SIES Graduate School of Technology

Nerul, Navi Mumbai

---

## Workshop Developed For

**Value Added Course**

**Modern DevOps and Industry Cloud Automation**

---

## Technologies Used

* Git
* GitHub
* GitHub Actions
* Amazon EC2
* Amazon S3
* Amazon DynamoDB
* IAM
* Ubuntu Linux
* Python
* Flask
* Gunicorn

---

# Section 14 – License

This project is released for educational purposes.

Students are encouraged to:

* Learn
* Experiment
* Modify
* Improve
* Share knowledge

Please provide appropriate attribution when using this material in academic or training environments.

---

# Final Project Architecture

```text
                           Developer
                                │
                          Modify Code
                                │
                                ▼
                           Git Commit
                                │
                                ▼
                            Git Push
                                │
                                ▼
                      GitHub Repository
                                │
                        Push Event Triggered
                                │
                                ▼
                    GitHub Actions Workflow
                 ┌──────────────────────────────┐
                 │                              │
                 ▼                              ▼
        Frontend Deployment             Backend Deployment
                 │                              │
        Upload Files to S3             SSH into EC2
                 │                              │
                 ▼                              ▼
        Amazon S3 Website              Git Pull
                                              │
                                              ▼
                                    Install Dependencies
                                              │
                                              ▼
                                      Restart Gunicorn
                                              │
                                              ▼
                                       Flask REST API
                                              │
                                              ▼
                                      Amazon DynamoDB
```

---

# Final Checklist

Before concluding the workshop, ensure the following tasks have been completed.

| Task                  | Completed |
| --------------------- | --------- |
| AWS Infrastructure    | ✅         |
| Amazon S3             | ✅         |
| EC2 Backend           | ✅         |
| DynamoDB              | ✅         |
| IAM Roles             | ✅         |
| IAM User              | ✅         |
| GitHub Repository     | ✅         |
| GitHub Secrets        | ✅         |
| Frontend Workflow     | ✅         |
| Backend Workflow      | ✅         |
| Automatic Deployment  | ✅         |
| End-to-End Testing    | ✅         |
| Deployment Validation | ✅         |


🎉 **Congratulations!**

You have successfully implemented a complete **Continuous Integration and Continuous Deployment (CI/CD) pipeline using GitHub Actions and Amazon Web Services (AWS).**

This project demonstrates real-world DevOps practices, including:

* Automated software delivery
* Infrastructure integration
* Secure authentication
* Cloud deployment
* Backend automation
* Frontend hosting
* Continuous deployment
* Monitoring and validation

Happy Learning and Happy Coding! 🚀
