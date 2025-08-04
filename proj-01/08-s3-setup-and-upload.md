# 🪣 proj-01/08 - S3 Setup and Upload (Source & Output)

## 🛠️ Project 1: CI/CD Pipeline with CodeBuild & CloudWatch Logs

This document captures the full setup of **Amazon S3 buckets** required for storing both the **source code** (`MessageUtil.zip`) and the **build artifacts** (`.jar` file output). It also walks through the upload and verification steps inside your AWS Console.

---

## 📍 Objective

1. Create two S3 buckets:
   - **Source Bucket** — holds the original zip file for build.
   - **Output Bucket** — stores the generated build artifact.

2. Upload the source file.
3. Validate settings like **blocking public access** and optionally enabling **versioning**.

---

## 🌐 Step-by-Step Instructions

### 🔹 Step 1: Login to AWS Console

- Go to: [https://console.aws.amazon.com/](https://console.aws.amazon.com/)
- Region: `us-east-1 (N. Virginia)`

---

### 🔹 Step 2: Create Source S3 Bucket

1. Navigate to **S3** → Click **Create Bucket**
2. **Bucket name**: `codebridge-source-bucket-eks2`
3. Region: `us-east-1`
4. Keep default settings
5. Ensure **Block Public Access** is enabled
6. Click **Create bucket**

---

### 🔹 Step 3: Upload MessageUtil.zip

1. Open the bucket: `codebridge-source-bucket-eks2`
2. Click **Upload**
3. Choose the file: `MessageUtil.zip`
4. Leave defaults
5. Click **Upload**

---

### 🔹 Step 4: Create Output Artifact Bucket

1. Go back to **S3** → Click **Create Bucket**
2. **Bucket name**: `codebridge-output-bucket-eks2`
3. Region: `us-east-1`
4. Under **Block Public Access**:
   - Ensure it's **enabled**
   - Acknowledge warning if shown
5. *(Optional but recommended)*: Enable **versioning**
6. Click **Create bucket**

---

## 📂 Bucket Summary

| Bucket Name                   | Purpose                     | Notes                           |
|------------------------------|-----------------------------|----------------------------------|
| `codebridge-source-bucket-eks2` | Stores original zip file     | Must match CodeBuild source input |
| `codebridge-output-bucket-eks2` | Stores build artifacts       | Must match CodeBuild output target |

---

## ✅ Final Checklist for This Stage

- [x] Source bucket created
- [x] Output bucket created
- [x] Zip file uploaded
- [ ] Confirmed correct region and naming
- [ ] Block Public Access enabled for both
- [ ] *(Optional)* Versioning enabled

---

## 📌 Notes

- These buckets are temporary for demo/lab use.
- In real production work, use naming conventions like:
  - `client-name-devops-source`
  - `client-name-prod-artifacts`

---

### ✍️ Created & Curated by  
**Muhammad Naveed Ishaque**  
_Content Creator | AI Writer | Narrative Simplifier_  
_With the inner voice of Eks2 — the whisper behind the work._  

**Siraat AI Academy**  
_“The Straight Path — Empowering minds with clarity, illuminating paths with purpose.”_
