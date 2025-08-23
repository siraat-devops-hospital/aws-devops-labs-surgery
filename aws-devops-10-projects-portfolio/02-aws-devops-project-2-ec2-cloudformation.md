# 🌟 Project 2: EC2 Deployment using CloudFormation

## 🎯 Goal
Launch EC2 instances with full setup via **Infrastructure as Code (IaC)** using AWS CloudFormation.

---

## 🗂 Repo Structure
```
aws-ec2-cloudformation/
├─ templates/
│   └─ ec2-template.yaml
├─ user-data.sh
└─ README.md
```

---

## 📜 CloudFormation Template (ec2-template.yaml)

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: EC2 instance launched via CloudFormation with UserData (Apache server)

Resources:
  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-08c40ec9ead489470   # Amazon Linux 2 (update region-specific AMI)
      KeyName: my-keypair              # replace with your key pair name
      SecurityGroupIds:
        - !Ref MySecurityGroup
      IamInstanceProfile: !Ref MyEC2InstanceProfile
      UserData:
        Fn::Base64: !Sub |
          #!/bin/bash
          yum update -y
          yum install -y httpd
          systemctl enable httpd
          systemctl start httpd
          echo "<h1>Hello from Eks2 CloudFormation 🚀</h1>" > /var/www/html/index.html

  MySecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Allow HTTP and SSH
      VpcId: vpc-xxxxxxxx    # replace with your default VPC ID
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 22
          ToPort: 22
          CidrIp: 0.0.0.0/0
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          CidrIp: 0.0.0.0/0

  MyEC2InstanceProfile:
    Type: AWS::IAM::InstanceProfile
    Properties:
      Roles:
        - !Ref MyEC2Role

  MyEC2Role:
    Type: AWS::IAM::Role
    Properties:
      AssumeRolePolicyDocument:
        Version: "2012-10-17"
        Statement:
          - Effect: Allow
            Principal:
              Service:
                - ec2.amazonaws.com
            Action:
              - sts:AssumeRole
      ManagedPolicyArns:
        - arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess

Outputs:
  InstanceId:
    Description: EC2 Instance ID
    Value: !Ref MyEC2Instance
  PublicIP:
    Description: Public IP of EC2
    Value: !GetAtt MyEC2Instance.PublicIp
```

---

## 📝 Steps to Execute

### 1) Prepare the Patient (Eks2 Style)
- Go to AWS Console → CloudFormation → Create stack.  
- Upload `ec2-template.yaml`.  
- Provide **stack name** (e.g., `eks2-ec2-stack`).  
- Replace placeholders (`vpc-xxxxxxxx`, `my-keypair`) with your values.  

### 2) Start the Surgery (Launch)
- Click **Next → Next → Create stack**.  
- CloudFormation will provision:
  - EC2 Instance (Amazon Linux 2)
  - Security Group (HTTP + SSH open)
  - IAM Role + Instance Profile

### 3) Recovery Check (Outputs)
- Once CREATE_COMPLETE, check **Outputs tab**:  
  - `PublicIP` → copy into browser  
  - You should see: **Hello from Eks2 CloudFormation 🚀**

### 4) Verify via SSH (Optional)
```bash
ssh -i my-keypair.pem ec2-user@<PublicIP>
curl localhost
```

### 5) Clean-Up (Discharge)
- Delete the stack from CloudFormation → resources (EC2, SG, IAM role) will be deleted automatically.  

---

## 🩺 Oxygen Mask (Common Issues)
- **Invalid KeyName** → Ensure keypair exists in same region.  
- **VPC ID mismatch** → Replace with your region’s default VPC.  
- **No internet / page not loading** → Check SG inbound rule for HTTP 80.  

---

## 📄 README Skeleton

```markdown
# EC2 Deployment with CloudFormation

## Goal
Provision an EC2 instance (Amazon Linux 2) with Apache installed, via CloudFormation.

## How to run
1. Go to CloudFormation → Create Stack.
2. Upload `ec2-template.yaml`.
3. Provide stack name, VPC ID, and key pair.
4. Wait until CREATE_COMPLETE.
5. Open EC2 public IP in browser → "Hello from Eks2 CloudFormation 🚀".

## Cleanup
Delete the stack → all resources removed automatically.
```

---

## 🎥 YouTube Flow
1. Intro: IaC = doctor’s prescription for AWS  
2. Walkthrough template (explain each resource like instruments)  
3. Deploy stack + check browser output  
4. Delete stack (show how everything cleans up like recovery)  

---

✍️ Created & Curated by **Muhammad Naveed Ishaque (Eks2)**  
_With AI as co-surgeon in this CloudFormation healing journey._
