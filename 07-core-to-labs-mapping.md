# 🔗 Mapping: Core Showcase Projects → 18 Detailed Labs  

This table shows how each **Core Showcase Project** overlaps with and prepares you for the **18 Detailed Projects**.  

---

## 🌟 Core Project 1: Dockerize + ECR/ECS Deployment  

**Covers concepts of:** Containers, IaC modularity, monitoring/logging, and IaC with Terraform.  

| Related Labs | Why it’s Connected |
|--------------|-------------------|
| #5 Docker → ECR → ECS | Direct overlap (same lifecycle) |
| #10 Nested Stacks | ECS + modular infra often built with IaC |
| #16 Centralized Logging | ECS → CloudWatch integration |
| #18 Terraform on AWS | Same infra concepts, different IaC tool |

---

## 🌟 Core Project 2: Serverless Workflow (Step Functions)  

**Covers concepts of:** Lambda, event-driven workflows, secrets, triggers, canary deployments.  

| Related Labs | Why it’s Connected |
|--------------|-------------------|
| #3 Lambda + S3 Trigger | Step Functions often start with Lambda triggers |
| #4 IAM + Secrets Manager | Secure Lambda access needed in workflows |
| #8 Step Functions Workflow | Direct overlap (state machine orchestration) |
| #15 Canary Deployment (Lambda) | Same traffic-shifting applied to Lambda |

---

## 🌟 Core Project 3: CI/CD with CodePipeline  

**Covers concepts of:** Build automation, approvals, rollback, monitoring, compliance, patching.  

| Related Labs | Why it’s Connected |
|--------------|-------------------|
| #1 CI/CD Pipeline | Direct overlap (pipeline orchestration) |
| #11 Blue/Green Deploy | Rollback + safe deployment strategy |
| #12 Multi-Region Failover | Pipeline can integrate DR testing |
| #13 Pipeline Approval | Manual approval stage = extension of pipeline |
| #14 Drift Detection | Compliance checks in CI/CD workflows |
| #17 Automated Patching | Ops automation fits in pipeline workflows |

---

## 🧭 Summary  

- **Core Project 1 (Dockerize + ECS)** → prepares you for infra + container-focused labs.  
- **Core Project 2 (Serverless Workflow)** → prepares you for event-driven, serverless, and Lambda-focused labs.  
- **Core Project 3 (CI/CD with CodePipeline)** → prepares you for automation, compliance, and deployment strategy labs.  

👉 Master these 3, and the 18 Labs become **familiar, faster, and easier**.  
