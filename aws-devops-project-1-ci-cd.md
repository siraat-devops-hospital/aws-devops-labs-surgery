# 🚀 Project 1: CI/CD Pipeline with CodeBuild + CodeDeploy + CodePipeline

## 🎯 Goal
Automate the build, test, and deploy process of a sample app to EC2.

---

## 🗂 Repo Structure
```
aws-cicd-ec2/
├─ app/index.html
├─ buildspec.yml
├─ appspec.yml
└─ scripts/
   ├─ before_install.sh
   ├─ after_install.sh
   ├─ start_server.sh
   └─ stop_server.sh
```

### Sample Files

**app/index.html**
```html
<!doctype html>
<html><body style="font-family:system-ui;">
<h1>Hello from Eks2 CI/CD 🚀</h1>
<p>Deployed by CodePipeline → CodeBuild → CodeDeploy</p>
</body></html>
```

**buildspec.yml**
```yaml
version: 0.2
phases:
  install:
    commands:
      - echo "Install phase"
  build:
    commands:
      - echo "Zipping artifact"
      - zip -r artifact.zip app appspec.yml scripts
artifacts:
  files:
    - artifact.zip
```

**appspec.yml**
```yaml
version: 0.0
os: linux
files:
  - source: app/
    destination: /var/www/html/
hooks:
  BeforeInstall:
    - location: scripts/before_install.sh
      timeout: 300
      runas: root
  AfterInstall:
    - location: scripts/after_install.sh
      timeout: 300
      runas: root
  ApplicationStart:
    - location: scripts/start_server.sh
      timeout: 300
      runas: root
  ApplicationStop:
    - location: scripts/stop_server.sh
      timeout: 300
      runas: root
```

**scripts/before_install.sh**
```bash
#!/bin/bash
set -e
yum update -y
yum install -y httpd
systemctl enable httpd
```

**scripts/after_install.sh**
```bash
#!/bin/bash
set -e
chown -R apache:apache /var/www/html
```

**scripts/start_server.sh**
```bash
#!/bin/bash
set -e
systemctl restart httpd
```

**scripts/stop_server.sh**
```bash
#!/bin/bash
set -e
systemctl stop httpd || true
```

---

## 🩺 Five Surgery Stages

### 1) Prepare
- Region: pick one (e.g., eu-north-1)
- Create S3 artifact bucket
- IAM Roles: CodePipeline, CodeBuild, CodeDeploy, EC2 instance profile

### 2) Launch EC2
- AMI: Amazon Linux 2, t2.micro
- SG: inbound HTTP 80 (+ SSH 22 optional)
- IAM Instance Profile: EC2 role
- Tag: `CodeDeploy=Demo`

Install CodeDeploy agent:
```bash
sudo yum update -y
sudo yum install -y ruby wget
cd /home/ec2-user
wget https://aws-codedeploy-<REGION>.s3.<REGION>.amazonaws.com/latest/install
chmod +x ./install
sudo ./install auto
sudo systemctl status codedeploy-agent
```

### 3) CodeDeploy
- Create Application & Deployment Group
- Filter: `CodeDeploy=Demo` tag

### 4) CodeBuild
- Source: GitHub repo
- Artifacts: S3 bucket

### 5) CodePipeline
- Source: GitHub branch
- Build: CodeBuild project
- Deploy: CodeDeploy group

Result: EC2 public IP shows "Hello from Eks2 CI/CD 🚀".

---

## 🧯 Troubleshooting
- **No instances found:** Tag mismatch or agent not running
- **AccessDenied:** EC2 role missing S3 read
- **Health check fails:** Apache not running
- **Pipeline stuck:** GitHub connection issue

---

## 🧹 Cleanup
Delete Pipeline, Build project, Deploy group, S3 bucket, EC2, IAM roles.

---

## 🎥 YouTube Flow
1. 30s – What + diagram
2. 60s – Repo tour
3. 90s – EC2 + CodeDeploy
4. 60s – Pipeline run
5. 30s – Troubleshoot + cleanup

---

✍️ Created & Curated by **Muhammad Naveed Ishaque (Eks2)**
