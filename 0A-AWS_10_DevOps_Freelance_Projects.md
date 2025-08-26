# 🚀 Top 10 AWS DevOps Freelance Projects — Execution Mode
_Built for delivery, not just theory._

**Purpose:** Learn-by-shipping projects you can demo to clients, document on GitHub, and present on YouTube.

**Last updated:** <!-- set via badge or commit date -->

## 🧰 Prerequisites
- AWS account (Owner for setup, then scoped down), default region (e.g., `eu-north-1`), AWS CLI v2 logged in
- Node.js or Python (for Lambda samples), Docker (for ECR/ECS), Git
- IAM user with programmatic access for CI where applicable

## 🛡️ Guardrails (read first)
- **Costs:** Most projects fit Free Tier if small. Always review pricing.  
- **Cleanup:** Each project includes a teardown script/section — **run it** after testing.  
- **Security:** Least-privilege IAM, no plaintext secrets, resource tagging `Project=DevOps-Top10`.

## 🗺️ How to use this repo
1) Pick a project from the table. 2) Open its folder → follow `README`. 3) Record a short demo.  
**Freelance bonus:** Each project includes acceptance criteria & a mini SOW snippet.

## 📊 Projects at a glance

| # | Project | AWS Services | Difficulty | Est. Cost* | Folder |
|---|---------|--------------|------------|------------|--------|
| 1 | CI/CD: CodeBuild → CodeDeploy → CodePipeline | CodeCommit/Hub, CodeBuild, CodeDeploy, EC2 | ⭐⭐ | Low | `/01-cicd-codepipeline/` |
| 2 | EC2 via CloudFormation (IaC) | CFN, EC2, SSM | ⭐ | Low | `/02-cfn-ec2/` |
| 3 | S3 → Lambda → SES notify | S3, Lambda, SES | ⭐⭐ | Low | `/03-s3-lambda-ses/` |
| 4 | Secrets Manager with Lambda | Lambda, Secrets Manager, IAM | ⭐⭐ | Low | `/04-lambda-secrets/` |
| 5 | Docker → ECR → ECS | ECR, ECS Fargate, CloudWatch | ⭐⭐⭐ | Low–Med | `/05-ecr-ecs/` |
| 6 | CloudWatch logs & alarms | CloudWatch, SNS | ⭐ | Low | `/06-cw-alarms/` |
| 7 | Config + GuardDuty | AWS Config, GuardDuty, S3 | ⭐⭐ | Low | `/07-config-guardduty/` |
| 8 | Step Functions workflow | Step Functions, Lambda | ⭐⭐ | Low | `/08-step-functions/` |
| 9 | Inspector for EC2 | Inspector, EC2, SSM | ⭐⭐ | Low | `/09-inspector/` |
|10 | Nested CloudFormation | CFN (nested) | ⭐⭐ | Low | `/10-cfn-nested/` |

\* _Costs depend on usage/region; tear down resources after demos._

## 🧾 Deliverable conventions
- `README.md` (intro, arch diagram, steps, cleanup)
- `architecture.mmd` (Mermaid diagram)
- `policy.json` (least-privilege)
- `pipeline.yml` / `template.yaml`
- `demo-script.md` (2–4 min talking points)
- `SOW-snippet.md` + `acceptance-criteria.md`
- `cleanup.sh` / `cleanup.ps1`

## 📝 License & support
- License: MIT (recommended)  
- Issues: open a GitHub Issue with repro steps and logs (no secrets).
