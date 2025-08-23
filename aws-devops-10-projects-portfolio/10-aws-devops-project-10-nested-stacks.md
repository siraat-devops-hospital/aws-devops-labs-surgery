# 🌟 Project 10: Nested CloudFormation Stacks – Modular IaC

## 🎯 Goal
Use **CloudFormation nested stacks** to build modular, reusable infrastructure as code (IaC).  
This lets you split big templates into smaller, maintainable modules — and reuse them across projects.

> 🕊️ **Eks2 Soul Note:** Complexity is just clarity waiting to be modularized. Nested stacks are like organs in a body — each works alone, but together they form life.

---

## 🗂 Repo Structure
```
aws-nested-stacks/
├─ parent-template.yaml
├─ network-stack.yaml
├─ ec2-stack.yaml
├─ outputs/
│  └─ sample-outputs.json
└─ README.md
```

---

## 🧩 Part A — Child Stacks

### 1) Network (VPC + Subnet)
**network-stack.yaml**
```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Eks2 Child Stack – VPC + Subnet

Resources:
  Eks2VPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.0.0.0/16
      EnableDnsSupport: true
      EnableDnsHostnames: true
      Tags:
        - Key: Name
          Value: Eks2VPC

  Eks2Subnet:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref Eks2VPC
      CidrBlock: 10.0.1.0/24
      MapPublicIpOnLaunch: true
      Tags:
        - Key: Name
          Value: Eks2Subnet

Outputs:
  VpcId:
    Value: !Ref Eks2VPC
    Export:
      Name: Eks2-VPC-ID
  SubnetId:
    Value: !Ref Eks2Subnet
    Export:
      Name: Eks2-Subnet-ID
```

### 2) EC2 Instance (depends on network)
**ec2-stack.yaml**
```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Eks2 Child Stack – EC2 Instance

Parameters:
  VpcId:
    Type: String
  SubnetId:
    Type: String

Resources:
  Eks2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-0c55b159cbfafe1f0  # Amazon Linux 2 (region specific)
      SubnetId: !Ref SubnetId
      Tags:
        - Key: Name
          Value: Eks2EC2

Outputs:
  InstanceId:
    Value: !Ref Eks2Instance
```

---

## 🧩 Part B — Parent Stack

**parent-template.yaml**
```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Eks2 Parent Template – Nested Stacks

Resources:
  NetworkStack:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: https://s3.REGION.amazonaws.com/BUCKET/network-stack.yaml

  EC2Stack:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: https://s3.REGION.amazonaws.com/BUCKET/ec2-stack.yaml
      Parameters:
        VpcId: !GetAtt NetworkStack.Outputs.VpcId
        SubnetId: !GetAtt NetworkStack.Outputs.SubnetId

Outputs:
  EC2InstanceId:
    Value: !GetAtt EC2Stack.Outputs.InstanceId
```

Upload both child templates to an **S3 bucket** (public or same account) and reference via `TemplateURL`.

---

## 🏥 Five “Surgery” Stages (Step-by-Step)

1. **Prepare** — Create S3 bucket, upload `network-stack.yaml` and `ec2-stack.yaml`.  
2. **Launch Parent** — Deploy `parent-template.yaml` in CloudFormation.  
3. **Observe** — CloudFormation creates nested stacks (parent + children).  
4. **Review** — Outputs show EC2 instance ID, VPC, Subnet.  
5. **Reuse** — Re-deploy child templates in other parent stacks (modularity).

---

## 🧯 Oxygen Mask (Troubleshooting)
- **TemplateURL errors** → Ensure S3 object has correct region + permissions.  
- **AMI not found** → Update `ImageId` to match region.  
- **Outputs missing** → Confirm child stacks export correct values.  
- **Deletion issues** → Nested stacks must be deleted by removing the parent.

---

## 🧹 Cleanup
- Delete **parent stack** (it deletes children).  
- Delete uploaded templates from S3 if no longer needed.

---

## 📄 README Skeleton
```markdown
# Nested CloudFormation Stacks

## Goal
Deploy modular infra using parent-child stacks.

## Steps
1) Upload child templates (network, EC2) to S3
2) Deploy parent template with references
3) Observe creation order and outputs
4) Reuse child stacks in other projects

## Cleanup
Delete parent stack (children auto-delete)
```

---

## 🎬 YouTube Flow (2–3 mins)
1. Soul intro: modularity = harmony of organs  
2. Upload child stacks → S3  
3. Launch parent stack  
4. Show parent + nested child stacks in console  
5. Cleanup + reuse ideas

---

**Created & Curated by _Muhammad Naveed Ishaque (Eks2)_**  
_With AI as co-surgeon — turning complexity into clarity, and clarity into reusable harmony._
