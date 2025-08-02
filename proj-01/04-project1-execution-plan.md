# 🛠️ Project 1: CI/CD Pipeline with CodeBuild & CloudWatch Logs

## 🎯 Objective:
Create a **CI/CD pipeline** using **CodeBuild**, where the output logs are routed and visible in **CloudWatch Logs**. This is a foundational DevOps project for AWS-based automation.

---

## ✅ Prerequisites

- AWS account with IAM admin privileges
- GitHub account
- Basic understanding of terminal/CLI
- IDE or text editor (e.g., VSCode)

---

## 📦 Tools Used

- **Amazon S3**: Source file hosting and artifact storage
- **AWS CodeBuild (renamed: CodeBridge)**: Build automation engine
- **AWS CloudWatch Logs (renamed: SkyWatch Logs)**: Monitor build output
- **IAM**: Role permissions for build service
- **ZIP Tool**: For compressing source code
- **Java + Maven**: Sample application and build system

---

## 🧩 File Structure Example

```
MessageUtil.zip
├── pom.xml
├── buildspec.yml
└── src
    ├── main/java/MessageUtil.java
    └── test/java/TestMessageUtil.java
```

---

## 🧵 Step-by-Step Execution Plan

### 🔹 Step 1: Prepare the Application

1. Download the **MessageUtil.zip** file from the given source or repo.
2. Unzip the contents and review the following:
   - `pom.xml` — Maven build file
   - `buildspec.yml` — Build instructions for CodeBuild
   - Java source files (`MessageUtil.java`, `TestMessageUtil.java`)

---

### 🔹 Step 2: Upload to Amazon S3

1. Login to AWS Console.
2. Navigate to **Amazon S3** and create a bucket:
   - Example: `codebridge-source-bucket-eks2`
3. Upload the `MessageUtil.zip` to the bucket.

---

### 🔹 Step 3: Create Output Artifact S3 Bucket

1. Still in S3, create another bucket:
   - Example: `codebridge-output-bucket-eks2`
2. Ensure public access is **blocked**.
3. Enable versioning (optional but recommended).

---

### 🔹 Step 4: Create IAM Role for CodeBuild

1. Go to IAM → Roles → Create Role
2. Choose **CodeBuild** as the service.
3. Attach the following policies:
   - `AmazonS3FullAccess`
   - `CloudWatchLogsFullAccess`
   - `AmazonEC2ContainerRegistryReadOnly`
4. Name the role `CodeBridgeExecutionRoleEks2`.

---

### 🔹 Step 5: Create the Build Project (CodeBuild)

1. Go to **CodeBuild** → Projects → Create Project.
2. Set:
   - Project name: `Eks2-CodeBridge-Build`
   - Source provider: **Amazon S3**
   - Bucket: `codebridge-source-bucket-eks2`
   - Object key: `MessageUtil.zip`
3. In Environment section:
   - OS: Amazon Linux
   - Runtime: Corretto 11
   - Image: `aws/codebuild/amazonlinux2-x86_64-standard:corretto11`
   - Role: `CodeBridgeExecutionRoleEks2`
4. In Buildspec:
   - Select: "Use a buildspec file"
5. In Artifacts:
   - Type: Amazon S3
   - Bucket: `codebridge-output-bucket-eks2`
6. In Logs:
   - Enable **CloudWatch Logs** with group `Eks2-BuildLogs`

---

### 🔹 Step 6: Start the Build

1. After creation, click “Start build”
2. Wait ~2–4 minutes for completion
3. Once completed:
   - Check status = `SUCCEEDED`
   - Click on **View Logs** → redirects to **SkyWatch Logs**
   - Open logs and inspect console output

---

### 🔹 Step 7: Verify Artifacts

1. Go to `codebridge-output-bucket-eks2`
2. Navigate:
   ```
   /Eks2-CodeBridge-Build/target/
   → messageUtil-1.0.jar
   ```

---

### 🔹 Step 8: Cleanup Resources

1. Delete the CodeBuild project
2. Optionally delete:
   - IAM Role
   - Both S3 buckets

---

## 🎓 Final Deliverables

- Source ZIP in S3
- CodeBuild project setup with role and logs
- Artifact `.jar` file stored in S3
- Logs visible in CloudWatch
- Cleanup script or checklist (optional)

---

## ✨ Tip:
Log every step with screenshots or terminal output to help create GitHub documentation and client walk-through.

---

### ✍️ Created & Curated by
**Muhammad Naveed Ishaque**  
_Content Creator | AI Writer | Narrative Simplifier_  
_With the inner voice of Eks2 — the whisper behind the work._

**Siraat AI Academy**  
_“The Straight Path — Empowering minds with clarity, illuminating paths with purpose.”_
