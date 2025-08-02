
# 🛠️ Tools for Project 1: Where to Use Code and Why

## 1. 🖥️ Visual Studio Code (VS Code)

### 🔧 Purpose:
- **Main environment** for writing, editing, and organizing files like `buildspec.yml`, `Dockerfile`, and IaC templates (`CloudFormation`, `Terraform`, etc.).
- **Extensions** like AWS Toolkit or YAML linting greatly enhance productivity.

### ✅ When to Use:
- Writing infrastructure-as-code (IaC) templates
- Drafting configuration files (`.yml`, `.json`)
- Editing `README.md`, documentation, or pipeline scripts
- Version control via Git extensions

---

## 2. 💻 Terminal / CLI (Command Line Interface)

### 🔧 Purpose:
- Direct **execution of commands** using AWS CLI, Git, or Docker
- **Testing, configuring, and deploying** the project quickly and efficiently

### ✅ When to Use:
- Cloning GitHub repos: `git clone ...`
- Using AWS CLI: `aws s3 cp`, `aws cloudformation deploy`, etc.
- Executing Docker commands: `docker build`, `docker push`
- Deploying infrastructure using CLI: `aws cloudformation create-stack`

---

## 3. 🔁 Integrated Terminal in VS Code

### 🔧 Purpose:
- Combines the **power of CLI with VS Code UI**
- Keeps your workflow within one window

### ✅ When to Use:
- Running Git commands alongside file edits
- Running build/deploy scripts directly from the repo folder
- Switching between IaC validation and file correction quickly

---

## 4. 🌐 AWS Console (Browser-Based)

### 🔧 Purpose:
- Visual monitoring of resources and deployments
- Accessing services like **CodePipeline**, **S3**, **CloudWatch**, etc.

### ✅ When to Use:
- Checking logs in CloudWatch
- Navigating S3 buckets and file structure
- Creating or reviewing CodeBuild or CodePipeline configurations
- Viewing CloudFormation stack events

---

## ✅ Summary

| Tool          | Why Use It                             | Where to Use                     |
|---------------|-----------------------------------------|----------------------------------|
| VS Code       | Edit files, write scripts, lint config  | Local Dev Workspace              |
| Terminal/CLI  | Run AWS & Git commands, build containers| Bash, Shell, PowerShell          |
| VS Code Terminal | Integrated file edits & commands     | Inside VS Code                   |
| AWS Console   | Visual deployment monitoring, dashboards| Browser                          |
