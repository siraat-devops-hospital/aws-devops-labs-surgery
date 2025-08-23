# 🌟 Project 7: AWS Config + GuardDuty Setup (Detect • Audit • Alert)

## 🎯 Goal
Enable **continuous auditing (AWS Config)** and **threat detection (Amazon GuardDuty)** with simple, repeatable steps — plus alerts to **SNS** and optional export of findings to **S3**.

> 🕊️ **Eks2 Soul Note:** Security is not fear — it’s awareness. After this, your cloud will *speak up* when something’s wrong.

---

## ⚠️ Cost Note (Honest)
- **GuardDuty** and **Config** are **paid** services after free trial thresholds. Keep resources minimal, run in a sandbox account/region, and **clean up** after the demo.

---

## 🗂 Repo Structure
```
aws-config-guardduty/
├─ config/
│  ├─ enable-config.sh
│  ├─ s3-bucket-policy.json
│  └─ managed-rules.json
├─ guardduty/
│  ├─ enable-guardduty.sh
│  ├─ eventbridge-rule-gd-findings.json
│  └─ s3-destination-policy.json
├─ sns/
│  └─ create-topic-subscribe.sh
└─ README.md
```

---

## 🧩 Part A — SNS Topic (one-time)

**sns/create-topic-subscribe.sh**
```bash
#!/usr/bin/env bash
set -euo pipefail
REGION="eu-north-1"
EMAIL="your.email@example.com"

TOPIC_ARN=$(aws sns create-topic --name eks2-security-alerts --region "$REGION" --query 'TopicArn' --output text)
aws sns subscribe --topic-arn "$TOPIC_ARN" --protocol email --notification-endpoint "$EMAIL" --region "$REGION"

echo "Topic: $TOPIC_ARN"
echo "👉 Check your email and CONFIRM the subscription."
```
> We’ll reuse this topic for both **Config** and **GuardDuty** notifications.

---

## 🧩 Part B — AWS Config (Audit & Compliance)

### 1) Create an S3 bucket for Config snapshots
- Bucket name must be globally unique, e.g., `eks2-config-logs-123456`.  
- Apply a **bucket policy** to allow AWS Config to write.

**config/s3-bucket-policy.json**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AWSConfigBucketPermissionsCheck",
      "Effect": "Allow",
      "Principal": { "Service": "config.amazonaws.com" },
      "Action": "s3:GetBucketAcl",
      "Resource": "arn:aws:s3:::REPLACE_BUCKET"
    },
    {
      "Sid": "AWSConfigBucketDelivery",
      "Effect": "Allow",
      "Principal": { "Service": "config.amazonaws.com" },
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::REPLACE_BUCKET/AWSLogs/ACCOUNT_ID/Config/*",
      "Condition": { "StringEquals": { "s3:x-amz-acl": "bucket-owner-full-control" } }
    }
  ]
}
```
Replace `REPLACE_BUCKET` and `ACCOUNT_ID`. Attach via S3 console → **Permissions → Bucket policy**.

### 2) Enable Config recorder + delivery channel
**config/enable-config.sh**
```bash
#!/usr/bin/env bash
set -euo pipefail
REGION="eu-north-1"
BUCKET="eks2-config-logs-123456"   # your bucket
ROLE_ARN=""                        # leave empty to let console create default role

# Create recorder (all resources + global resources)
aws configservice put-configuration-recorder   --configuration-recorder name="default",roleARN="$ROLE_ARN",recordingGroup="{"allSupported":true,"includeGlobalResourceTypes":true}"   --region "$REGION"

# Delivery channel to S3
aws configservice put-delivery-channel   --delivery-channel "name=default,s3BucketName=$BUCKET"   --region "$REGION"

# Start recording
aws configservice start-configuration-recorder --configuration-recorder-name default --region "$REGION"

echo "✅ AWS Config enabled. Recording to s3://$BUCKET"
```
> If `ROLE_ARN` is empty, doing this in **console** is easier (it creates the service-linked role automatically).

### 3) Add a few managed rules (JSON sample)
**config/managed-rules.json**
```json
{
  "rules": [
    { "name": "s3-bucket-public-read-prohibited", "source": "AWS" },
    { "name": "root-account-mfa-enabled", "source": "AWS" },
    { "name": "ec2-volume-inuse-check", "source": "AWS" },
    { "name": "restricted-ssh", "source": "AWS" }
  ]
}
```
Create these via console → **AWS Config → Rules → Add rule** and search the names above.

> **Tip:** Set rule **scope** to “All resources” unless you need filtering. For demo, keep it simple.

### 4) Alerts for Noncompliant rules (via EventBridge → SNS)
- Go to **EventBridge → Rules → Create rule**  
- Event pattern (Service: **AWS Config**, event type: **Config Rules Compliance Change**)  
- Target: **SNS topic** `eks2-security-alerts`  
Now whenever a resource becomes **NON_COMPLIANT**, you get an email.

---

## 🧩 Part C — Amazon GuardDuty (Threat Detection)

### 1) Enable GuardDuty
**guardduty/enable-guardduty.sh**
```bash
#!/usr/bin/env bash
set -euo pipefail
REGION="eu-north-1"

DETECTOR_ID=$(aws guardduty create-detector --enable --region "$REGION" --query 'DetectorId' --output text 2>/dev/null || true)
if [ "$DETECTOR_ID" = "null" ] || [ -z "$DETECTOR_ID" ]; then
  DETECTOR_ID=$(aws guardduty list-detectors --region "$REGION" --query 'DetectorIds[0]' --output text)
fi
echo "Detector: $DETECTOR_ID"

# (Optional) Enable all features for new accounts — console usually does this by default.
echo "✅ GuardDuty enabled in $REGION"
```

### 2) Route high-severity findings to email (EventBridge → SNS)
**guardduty/eventbridge-rule-gd-findings.json**
```json
{
  "source": ["aws.guardduty"],
  "detail-type": ["GuardDuty Finding"],
  "detail": {
    "severity": [ { "numeric": [">=", 5] } ]
  }
}
```
Create rule via console:
- Name: `eks2-guardduty-severity5plus`
- Event pattern: JSON above (paste into EventBridge)
- Target: SNS → `eks2-security-alerts`

> Start with severity `>= 5` (medium+) to avoid noise. Adjust later.

### 3) (Optional) Export findings to S3
You can export findings to S3 using **GuardDuty publishing destination**. Requires a bucket + access policy.
**guardduty/s3-destination-policy.json**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowGuardDutyToWrite",
      "Effect": "Allow",
      "Principal": { "Service": "guardduty.amazonaws.com" },
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::REPLACE_BUCKET/AWSLogs/ACCOUNT_ID/GuardDuty/*",
      "Condition": { "StringEquals": { "s3:x-amz-acl": "bucket-owner-full-control" } }
    }
  ]
}
```
Then (CLI example):
```bash
aws guardduty create-publishing-destination   --detector-id DETECTOR_ID   --destination-arn arn:aws:s3:::REPLACE_BUCKET   --destination-type S3   --region eu-north-1
```

---

## 🏥 Five “Surgery” Stages (Step-by-Step)

1) **Prepare** — Create SNS topic & confirm email. Create S3 bucket for Config.  
2) **Enable Config** — Recorder + delivery channel; add a few **managed rules**.  
3) **Enable GuardDuty** — Detector on; create EventBridge rule for severity ≥ 5 findings → SNS.  
4) **Generate Signals** — Make a safe misconfig (e.g., open SSH from 0.0.0.0/0 on a test SG) to see Config **NON_COMPLIANT**.  
5) **Verify & Review** — Check **Config rule evaluations** and **GuardDuty findings**; confirm email alerts.

---

## 🧯 Oxygen Mask (Troubleshooting)

- **No Config snapshots in S3** → Bucket policy missing `PutObject` with `bucket-owner-full-control`; or recorder not started.  
- **No emails** → SNS subscription not confirmed; EventBridge target wrong region/topic.  
- **GuardDuty quiet** → Severity filter too high; wait a few minutes; generate benign findings (e.g., use known test IPs if allowed in docs).  
- **Permissions errors** → Prefer console to let AWS create service-linked roles automatically (fast for demos).

---

## 🧹 Cleanup
- **Config**: Stop recorder → delete recorder + delivery channel → (optionally) delete S3 bucket data.  
- **GuardDuty**: Delete EventBridge rule/target → optionally disable/delete detector → remove S3 publishing destination.  
- **SNS**: Unsubscribe → delete topic.  
- Remove test resources (e.g., public SSH SG) you created to trigger alerts.

---

## 📄 README Skeleton
```markdown
# AWS Config + GuardDuty (Audit + Threat Detection)

## Goal
Continuously audit resources and detect threats. Send alerts to SNS and (optionally) export findings to S3.

## Steps
1) Create SNS topic + confirm email
2) Enable AWS Config (recorder + channel) → add 3–4 managed rules
3) Enable GuardDuty → EventBridge rule for severity >= 5 → SNS
4) Generate a safe misconfig / test → receive emails
5) Review findings, then clean up

## Cleanup
Stop/delete Config components, disable GuardDuty, delete EventBridge rule, SNS topic, and test resources.
```

---

## 🎬 YouTube Flow (2–3 mins)
1. Why security = awareness (soul note)  
2. Config enable + managed rules  
3. GuardDuty enable + EventBridge rule  
4. Trigger + see alert emails  
5. Cleanup + next steps (Security Hub)

---

**Created & Curated by _Muhammad Naveed Ishaque (Eks2)_**  
_With AI as co-surgeon — turning silence into signals, and signals into safety._
