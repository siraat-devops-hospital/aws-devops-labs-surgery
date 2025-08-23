# 🌟 Project 4: IAM Roles + Secrets Manager Integration (Lambda Secure Access)

## 🎯 Goal
Use **AWS Secrets Manager** to store a secret (e.g., API key / DB password) and access it securely from **AWS Lambda** using the right **IAM role** — no hard-coded credentials.

---

## 🗂 Repo Structure
```
aws-lambda-secrets-manager/
├─ lambda/
│  └─ handler.py
├─ policy/
│  └─ lambda-secrets-policy.json
├─ templates/ (optional CFN)
│  └─ sample-secret.json
└─ README.md
```

---

## 🧠 How It Works
```
Secrets Manager (store secret)  →  IAM Role (allow GetSecretValue)  →  Lambda (fetch at runtime)
```
- You store a secret once (JSON or plain string).  
- Lambda assumes an execution role with **least privilege** to retrieve that specific secret.  
- On invocation, Lambda fetches the secret → uses it (e.g., call API) → returns safely.

---

## 🔐 Create a Secret (example)
1) Go to **Secrets Manager → Store a new secret**  
2) Secret type: **Other type of secret**  
3) **Key/value** (example JSON):  
```json
{
  "API_KEY": "super-secret-123",
  "ENDPOINT": "https://api.example.com"
}
```
4) Name: `eks2/demo/api-secret` (your choice)  
5) Region: keep consistent (e.g., `eu-north-1`)  
6) Finish (you will get a **Secret ARN**)  

Optional template (`templates/sample-secret.json`) for reference.

---

## 🐍 Lambda Function (Python 3.11)

**lambda/handler.py**
```python
import os
import json
import boto3
from botocore.exceptions import ClientError

REGION = os.getenv("AWS_REGION", "eu-north-1")
SECRET_NAME = os.getenv("SECRET_NAME", "eks2/demo/api-secret")

secrets = boto3.client("secretsmanager", region_name=REGION)

def get_secret_json(secret_name: str):
    try:
        resp = secrets.get_secret_value(SecretId=secret_name)
        if "SecretString" in resp:
            return json.loads(resp["SecretString"])
        else:
            # binary not expected here, but handle gracefully
            import base64
            return json.loads(base64.b64decode(resp["SecretBinary"]).decode("utf-8"))
    except ClientError as e:
        # Log and bubble up for CloudWatch diagnostics
        print(f"[ERROR] get_secret_value failed: {e}")
        raise

def lambda_handler(event, context):
    # Fetch secret at runtime (no hard-coded creds)
    secret = get_secret_json(SECRET_NAME)

    api_key = secret.get("API_KEY")
    endpoint = secret.get("ENDPOINT", "https://example.com")

    # (Demo) pretend to call an external API
    message = {
        "message": "Secret fetched securely.",
        "endpoint": endpoint,
        "api_key_preview": f"{api_key[:3]}***" if api_key else None
    }
    print("[INFO] Using secret to perform work:", message)

    return {
        "statusCode": 200,
        "body": json.dumps(message)
    }
```

> **Why fetch per-invocation?**  
> - No plaintext in code or env vars.  
> - If the secret rotates, Lambda gets fresh values automatically.

---

## 🔏 IAM Least-Privilege Policy

**policy/lambda-secrets-policy.json**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowSpecificSecretRead",
      "Effect": "Allow",
      "Action": [
        "secretsmanager:GetSecretValue",
        "secretsmanager:DescribeSecret"
      ],
      "Resource": "arn:aws:secretsmanager:REGION:ACCOUNT_ID:secret:eks2/demo/api-secret-*"
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
Replace **REGION** and **ACCOUNT_ID**, or simply set `Resource` to your secret’s exact ARN.  
Attach this JSON as an **inline policy** to your Lambda **execution role**, with **AWSLambdaBasicExecutionRole** managed policy for logs.

> **Tip:** Scope the `Resource` to a **single secret ARN** — not `"*"` — for real projects.

---

## 🏥 Five “Surgery” Stages (step-by-step)

### 1) Prepare the Instruments
- Create the **secret** in Secrets Manager (note its **ARN** and **name**).  
- Create a **Lambda execution role** with:  
  - Managed: `AWSLambdaBasicExecutionRole`  
  - Inline: `lambda-secrets-policy.json` (use exact secret ARN)

### 2) Deploy Lambda
- Create function → Runtime **Python 3.11**.  
- Upload `handler.py`.  
- Set **Environment variables**:  
  - `AWS_REGION=eu-north-1` (or your region)  
  - `SECRET_NAME=eks2/demo/api-secret` (or your name)  
- Timeout: **10s** is fine for demo.

### 3) Invoke & Verify
- **Test** from console (any sample event).  
- Open **CloudWatch Logs** → verify: `Secret fetched securely.`  
- Response shows masked preview of API key + endpoint.

### 4) (Optional) Wire an Event
- API Gateway → Lambda (to test via HTTP)  
- EventBridge rule (timed refresh / scheduled tasks)  
- S3/Lambda chain (use secret for downstream call)

### 5) Clean-up
- Delete Lambda **if demo-only**.  
- Consider **keeping the secret** for reuse; else delete from Secrets Manager.  
- Remove inline IAM policy / role if created only for demo.

---

## 🧯 Oxygen Mask (Troubleshooting)
- **AccessDenied: GetSecretValue** → IAM policy resource doesn’t match secret ARN; fix region/account/secret name.  
- **Decryption failure** → Secret uses custom KMS CMK; role needs `kms:Decrypt` for that key.  
- **Throttling / Region mismatch** → Keep Lambda and Secrets Manager in **same region**.  
- **Costs** → Secrets Manager has a small monthly charge per secret; clean up when done.

---

## 🔁 Secret Rotation (Advanced)
Enable **automatic rotation** in Secrets Manager and point to a **rotation Lambda**. Your app Lambda doesn’t change — it always calls `GetSecretValue()` to get fresh data.

---

## 📄 README Skeleton
```markdown
# Lambda + Secrets Manager (Secure Access)

## Goal
Access a secret from Lambda without hard-coded credentials.

## Steps
1) Create a secret in Secrets Manager (JSON or string)
2) Create Lambda role (logs + GetSecretValue on that secret ARN)
3) Deploy Lambda (Python 3.11) with env: AWS_REGION, SECRET_NAME
4) Test invoke → check CloudWatch Logs

## Troubleshooting
- AccessDenied → fix ARN in IAM policy
- KMS CMK → add kms:Decrypt on key
- Region mismatch → keep services aligned
```

---

## 🎬 YouTube Flow (2–3 mins)
1. Why secrets matter (no creds in code)  
2. Store secret → show JSON  
3. Role & least-priv policy (one secret ARN)  
4. Lambda invoke → logs proof  
5. Cleanup + next steps (rotation)

---

**Created & Curated by _Muhammad Naveed Ishaque (Eks2)_**  
_With AI as co-surgeon — from fear to secure flow, one secret at a time._
