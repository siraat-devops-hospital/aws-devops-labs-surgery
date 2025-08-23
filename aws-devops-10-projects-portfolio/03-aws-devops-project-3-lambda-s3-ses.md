# 🌟 Project 3: Lambda + S3 Event Trigger → SES Email

## 🎯 Goal
When a file is uploaded to **S3**, trigger **AWS Lambda** to send an email via **Amazon SES**.  
This is a classic automation: perfect for receipts, upload confirmations, or alerting.

---

## 🗂 Repo Structure
```
aws-lambda-s3-ses/
├─ lambda/
│  └─ handler.py
├─ policy/
│  └─ lambda-inline-policy.json
├─ .env.sample
└─ README.md
```

### `.env.sample`
```
FROM_EMAIL=verified_sender@example.com
TO_EMAIL=verified_receiver@example.com
AWS_REGION=eu-north-1
```

> **Note (SES Sandbox):** In new AWS accounts, SES is usually in **sandbox mode**. You must **verify both sender and recipient emails** before emails will deliver. (Moving to production requires a support request.)

---

## 🧠 How it Works (High-Level)
```
S3 (ObjectCreated)  →  Lambda (reads event)  →  SES (send_email)
```
- You upload **any file** to the chosen S3 bucket.
- S3 event triggers Lambda with the object key + bucket name.
- Lambda formats a simple message and asks SES to send an email.

---

## 🧩 Lambda Function (Python 3.11)

**lambda/handler.py**
```python
import os
import urllib.parse
import boto3

SES_REGION = os.getenv("AWS_REGION", "eu-north-1")
FROM_EMAIL = os.getenv("FROM_EMAIL")
TO_EMAIL = os.getenv("TO_EMAIL")

ses = boto3.client("ses", region_name=SES_REGION)

def lambda_handler(event, context):
    # Expecting S3 put event
    records = event.get("Records", [])
    if not records:
        return {"status": "no-records"}

    for rec in records:
        if rec.get("eventSource") != "aws:s3":
            continue

        bucket = rec["s3"]["bucket"]["name"]
        key = urllib.parse.unquote_plus(rec["s3"]["object"]["key"])
        size = rec["s3"]["object"].get("size", "unknown")

        subject = f"[S3 Upload] {key} uploaded to {bucket}"
        body = (
            f"Hello from Eks2 Automation!\n\n"
            f"A new object was uploaded to your bucket.\n"
            f"Bucket: {bucket}\n"
            f"Key: {key}\n"
            f"Size: {size} bytes\n\n"
            f"— Sent automatically by Lambda + SES"
        )

        # Send email (simple text format)
        ses.send_email(
            Source=FROM_EMAIL,
            Destination={"ToAddresses": [TO_EMAIL]},
            Message={
                "Subject": {"Data": subject, "Charset": "UTF-8"},
                "Body": {"Text": {"Data": body, "Charset": "UTF-8"}},
            },
        )

    return {"status": "ok", "processed": len(records)}
```

> **Why Python?** It’s tiny, built-in `boto3` is available in the Lambda runtime, and the event parsing is super clear for learners.

---

## 🔐 IAM Permissions (Least-ish Privilege for Demo)

**policy/lambda-inline-policy.json**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowSES",
      "Effect": "Allow",
      "Action": ["ses:SendEmail", "ses:SendRawEmail"],
      "Resource": "*"
    },
    {
      "Sid": "AllowLogs",
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": "*"
    }
  ]
}
```

Attach this as an **inline policy** to the **Lambda execution role**, plus **AWSLambdaBasicExecutionRole** managed policy for logs (if not included already).

> We do **not** need broad S3 access since the event payload already contains object info. If you plan to **read the object content**, add `s3:GetObject` for that bucket ARN.

---

## 🏥 Five “Surgery” Stages (Step-by-Step)

### 1) Prepare the Instruments
1. **Create S3 bucket** (unique name, same region you’ll use for Lambda & SES).  
2. **Verify SES emails** (sandbox mode):  
   - Verify **FROM_EMAIL** and **TO_EMAIL** in **SES → Verified identities**.  
3. **Create Lambda execution role** with:  
   - Managed: `AWSLambdaBasicExecutionRole`  
   - Inline: `lambda-inline-policy.json` (SES send permissions)

### 2) Deploy the Lambda
1. Create function → Runtime **Python 3.11**.  
2. Upload `handler.py` (zip or inline).  
3. Set **Environment variables**: `FROM_EMAIL`, `TO_EMAIL`, `AWS_REGION`.  
4. Increase timeout to **30 seconds** (safe margin).  

### 3) Wire S3 → Lambda (Event Trigger)
1. In **S3 bucket → Properties → Event notifications**:  
   - Event name: `on-upload`  
   - Event types: `PUT` (ObjectCreated:Put)  
   - Destination: **Lambda function** (select it)  
2. Confirm/add **resource-based permission** allowing S3 to invoke Lambda (console does this automatically).

### 4) Test the Flow
- Upload a small file to S3 (e.g., `hello.txt`).  
- Watch **CloudWatch Logs** for Lambda.  
- Check your **TO_EMAIL** inbox. (In sandbox, both sender/receiver must be verified.)

### 5) Clean-up
- Remove S3 event notification.  
- Delete Lambda (if demo only).  
- Keep verified identities if you’ll reuse later.

---

## 🧯 Oxygen Mask (Troubleshooting)

- **No email received (SES 200 OK, but inbox empty):**  
  Likely **SES sandbox** + recipient not verified. Verify both emails or request production access.  

- **Lambda InvokePermission error from S3:**  
  Re-add the event notification from the bucket UI (it creates the permission automatically).  

- **Throttling / Region mismatch:**  
  Keep **S3, Lambda, SES** in the **same region** (e.g., `eu-north-1`).  

- **AccessDenied for logs:**  
  Ensure the role has `AWSLambdaBasicExecutionRole` or the inline logs permissions above.

---

## 🧪 Optional: Read Object Metadata or Content
To fetch object content inside Lambda:
```python
import boto3, os
s3 = boto3.client("s3")
obj = s3.get_object(Bucket=bucket, Key=key)
data = obj["Body"].read()  # bytes
```
> Add IAM permission: `s3:GetObject` for `arn:aws:s3:::YOUR_BUCKET_NAME/*`

---

## 📄 README Skeleton (drop-in)
```markdown
# Lambda + S3 Upload Trigger → SES Email

## Goal
Send an email each time a file is uploaded to S3 using Lambda + Amazon SES.

## Steps
1) Create S3 bucket (same region for SES/Lambda)
2) Verify sender & receiver emails in SES (sandbox)
3) Create Lambda role (logs + ses:SendEmail)
4) Deploy function (Python 3.11), set env: FROM_EMAIL, TO_EMAIL, AWS_REGION
5) Add S3 Event Notification → Lambda (ObjectCreated)
6) Upload file → receive email

## Troubleshooting
- SES sandbox: verify both emails
- Check CloudWatch Logs for errors
```

---

## 🎬 YouTube Flow (2–3 mins)
1. 15s Why: “auto email on upload” use-cases  
2. 45s Code tour (`handler.py`)  
3. 60s SES verify + S3 event wiring  
4. 30s Live upload → email received  
5. 20s Troubleshoot tips + cleanup

---

**Created & Curated by _Muhammad Naveed Ishaque (Eks2)_**  
_With AI as co-surgeon: turning fear into flow, one trigger at a time._
