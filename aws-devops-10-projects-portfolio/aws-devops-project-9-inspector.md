# 🌟 Project 9: Amazon Inspector – EC2 Vulnerability Scan

## 🎯 Goal
Enable **Amazon Inspector**, connect it to an EC2 instance, and detect vulnerabilities with a sample scan report.

> 🕊️ **Eks2 Soul Note:** Security is not punishment — it is awareness. After this, your EC2 will whisper its hidden weaknesses, and you will walk into the market with confidence, not fear.

---

## ⚠️ Cost Note (Honest)
Amazon Inspector is **paid** (per EC2/ECR resource). Free trial is available for first 15 days in new accounts. Run minimal tests and clean up.

---

## 🗂 Repo Structure
```
aws-inspector-ec2/
├─ setup/
│  ├─ enable-inspector.sh
│  └─ attach-iam-role.json
├─ reports/
│  └─ sample-findings.json
├─ cleanup/
│  └─ disable-inspector.sh
└─ README.md
```

---

## 🧩 Part A — Prepare EC2 Target

1. Launch a **t2.micro** EC2 (Amazon Linux 2, free tier).  
2. Attach an IAM role with **AmazonSSMManagedInstanceCore** policy.  
   - This allows Inspector to scan the instance via SSM.  
3. Ensure the instance is running in the same region where Inspector is enabled.

**setup/attach-iam-role.json**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ssm:DescribeInstanceInformation",
        "ssm:SendCommand",
        "ssm:GetCommandInvocation"
      ],
      "Resource": "*"
    }
  ]
}
```

---

## 🧩 Part B — Enable Amazon Inspector

**setup/enable-inspector.sh**
```bash
#!/usr/bin/env bash
set -euo pipefail
REGION="eu-north-1"

# Enable Inspector (if not already enabled)
aws inspector2 enable --resource-types EC2,ECR --region "$REGION"

# Verify status
aws inspector2 list-account-status --region "$REGION"
```

Console path: **Amazon Inspector → Enable → Resource types: EC2, ECR**.  
Inspector will auto-discover your EC2 instances.

---

## 🧩 Part C — Trigger Findings

- Once enabled, Inspector scans **automatically**.  
- For demo: Install an outdated package to generate a finding.  
```bash
sudo yum install -y httpd-2.2.15
```

- Wait 15–20 minutes. Findings will appear in Inspector console:  
  **Amazon Inspector → Findings**.

---

## 🧩 Part D — Review Sample Findings

Export a sample to JSON:  
```bash
aws inspector2 list-findings --region eu-north-1 --max-results 5 > reports/sample-findings.json
cat reports/sample-findings.json | jq .
```

Inside you’ll see:  
- **Title** (e.g., “Outdated Apache HTTP Server”)  
- **Severity** (LOW/MEDIUM/HIGH/CRITICAL)  
- **Remediation** (update package version)  

---

## 🏥 Five “Surgery” Stages (Step-by-Step)

1. **Prepare** — Launch EC2, attach IAM role.  
2. **Enable** — Turn on Amazon Inspector for EC2.  
3. **Generate** — Install outdated software to trigger findings.  
4. **Review** — Explore findings in console + export JSON.  
5. **Clean Up** — Stop Inspector, delete test EC2, remove roles.

---

## 🧯 Oxygen Mask (Troubleshooting)
- **No findings after 1h** → Ensure IAM role has SSM + Inspector enabled in correct region.  
- **AccessDenied** → Add `AmazonInspector2FullAccess` to IAM role/user for setup.  
- **Billing concern** → Remember Inspector is per-resource. Delete resources after demo.

---

## 🧹 Cleanup

**cleanup/disable-inspector.sh**
```bash
#!/usr/bin/env bash
set -euo pipefail
REGION="eu-north-1"

# Disable Inspector for EC2/ECR
aws inspector2 disable --resource-types EC2,ECR --region "$REGION"
```

Also terminate EC2 and detach/delete IAM roles.

---

## 📄 README Skeleton
```markdown
# Amazon Inspector EC2 Scan

## Goal
Scan EC2 for vulnerabilities and export a sample findings report.

## Steps
1) Launch EC2 (Amazon Linux 2)
2) Enable Inspector (resource type = EC2)
3) Install outdated software to trigger findings
4) Review findings in console and export JSON
5) Clean up

## Cleanup
Disable Inspector + terminate EC2 + delete roles.
```

---

## 🎬 YouTube Flow (2–3 mins)
1. Intro: why vulnerability scan matters (soul note)  
2. Launch EC2 + attach IAM role  
3. Enable Inspector + generate findings  
4. Show console findings + export JSON  
5. Cleanup + link to Security Hub / GuardDuty next

---

**Created & Curated by _Muhammad Naveed Ishaque (Eks2)_**  
_With AI as co-surgeon — shining light on the unseen, so fear turns into confidence._
