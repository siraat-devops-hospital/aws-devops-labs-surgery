# 📁 proj-01/00 - Execution Checklist

## 🛠️ Project 1: CI/CD Pipeline with CodeBuild & CloudWatch Logs

This is the official execution checklist for Project 1. It serves as a step-by-step verification log to track your progress, ensure readiness, and complete all DevOps stages smoothly and professionally.

---

## ✅ Execution Checklist

### 🔧 Preparation Stage
- [ ] **Download** `MessageUtil.zip`
- [ ] **Unzip & Verify Contents:**
  - `pom.xml`
  - `buildspec.yml`
  - `src/main/java/MessageUtil.java`
  - `src/test/java/TestMessageUtil.java`

---

### ☁️ AWS Setup Phase
- [ ] **Login to AWS Console** (Region: `us-east-1`)
- [ ] **Create Source S3 Bucket** (e.g., `codebridge-source-bucket-eks2`)
- [ ] **Upload** `MessageUtil.zip` to source bucket

---

### 📦 Artifact Bucket
- [ ] **Create Output S3 Bucket** (e.g., `codebridge-output-bucket-eks2`)
- [ ] **Block Public Access**
- [ ] *(Optional)* Enable **Versioning**

---

### 🛡️ IAM Role Setup
- [ ] **Create IAM Role:** `CodeBridgeExecutionRoleEks2`
- [ ] **Attach Policies:**
  - `AmazonS3FullAccess`
  - `CloudWatchLogsFullAccess`
  - `AmazonEC2ContainerRegistryReadOnly`

---

### 🏗️ CodeBuild Configuration
- [ ] **Create Build Project:** `Eks2-CodeBridge-Build`
- [ ] **Source Provider:** Amazon S3
- [ ] **Use Zip File:** `MessageUtil.zip`
- [ ] **Environment:**
  - OS: Amazon Linux 2
  - Runtime: Corretto 11
  - Image: `aws/codebuild/amazonlinux2-x86_64-standard:corretto11`
  - IAM Role: `CodeBridgeExecutionRoleEks2`
- [ ] **BuildSpec:** Use file from zip
- [ ] **Artifacts:** S3 → `codebridge-output-bucket-eks2`
- [ ] **Enable CloudWatch Logs** (e.g., `Eks2-BuildLogs`)

---

### ▶️ Build & Monitor
- [ ] **Start Build**
- [ ] **Wait for Completion**
- [ ] **Confirm Status: SUCCEEDED**
- [ ] **View Logs** in SkyWatch (CloudWatch)

---

### 📁 Validate Output
- [ ] **Check Artifact:**  
  Path: `Eks2-CodeBridge-Build/target/messageUtil-1.0.jar`  
  Location: `codebridge-output-bucket-eks2`

---

### 🧹 Cleanup (Post-Build)
- [ ] **Delete Build Project**
- [ ] *(Optional)* Delete IAM Role & S3 Buckets

---

### 🧘 Optional Enhancements
- [ ] Record **Walkthrough Video**
- [ ] Push **Documentation to GitHub**
- [ ] Prepare **Client Delivery Assets**

---

### ✍️ Created & Curated by  
**Muhammad Naveed Ishaque**  
_Content Creator | AI Writer | Narrative Simplifier_  
_With the inner voice of Eks2 — the whisper behind the work._

**Siraat AI Academy**  
_“The Straight Path — Empowering minds with clarity, illuminating paths with purpose.”_

_2025-08-02_
