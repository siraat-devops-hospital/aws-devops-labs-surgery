# 🌷 Lab 01 – CodeBuild & CloudWatch: Healing Through the Console

---

## 🌸 Stage 1 – What This Lab Aims to Heal 🎯

In this first operation, our patient is **"CodeBuild Anxiety"** — the fear of setting up a build pipeline from scratch.  
This lab walks you gently through the journey of creating your very first AWS CodeBuild project and seeing the live logs flow like **oxygen** through CloudWatch.

By the end, you will:

- Understand the structure of real-world Java project components (pom.xml, buildspec.yml)
- Create a CodeBuild project linked to an S3 source
- Store build artifacts safely
- View logs and outputs in CloudWatch
- Leave behind the fear of AWS console navigation

> 💖 This lab is a fresh breath for any layman wondering, *“Can I really do DevOps?”*  
Yes. And we’ll show you — gently, with clarity.

---

## 🛠️ Stage 2 – How to Complete This Lab (Procedure) 🩺

### ✨ Step-by-step Procedure:

1. **Sign into AWS Console**  
   - Use the given IAM credentials  
   - Select **us-east-1** (N. Virginia) as your region

2. **Explore the Sample Project**  
   - Navigate to **S3**, locate the `MessageUtil.zip` file in the `whizlabs-<random>` bucket  
   - Download and unzip it — observe:  
     - `pom.xml` – for Maven  
     - `buildspec.yml` – for CodeBuild  
     - `MessageUtil.java`, `TestMessageUtil.java` – source and test files

3. **Create an Output S3 Bucket**  
   - Name it something like `codebuild-whiz-output-{yourname}`  
   - Uncheck “Block all public access” and acknowledge the change

4. **Create the CodeBuild Project**  
   - Name: `WhizDemo`  
   - Source: `Amazon S3`, point to `MessageUtil.zip`  
   - Environment: Managed Image → Amazon Linux → Corretto11  
   - BuildSpec: Use `buildspec.yml`  
   - Artifacts: Store in your created output bucket  
   - Logging: Enable CloudWatch logs

5. **Run the Build**  
   - Click **Start build**  
   - Wait 2–5 mins until status shows: ✅ `SUCCEEDED`  
   - View full logs via CloudWatch  
   - Check artifact in S3 bucket → `WhizDemo/target/messageUtil-1.0.jar`

6. **Clean Up**  
   - Delete the build project from CodeBuild after completion  
   - Sign out from AWS Console

---

## ⚙️ Stage 3 – Tools Involved in This Operation 🛠

| Tool | Role |
|------|------|
| **AWS CodeBuild** | Builds source code using defined specs |
| **AWS S3** | Stores source and output artifacts |
| **AWS CloudWatch** | Displays logs and build statuses |
| **buildspec.yml** | Defines the build instructions |
| **pom.xml** | Maven config for the Java project |

---

## 🌍 Stage 4 – Real-World Healing (Job Use Case) 🌱

> This lab simulates what DevOps engineers do every day:  
**Automate builds, track logs, and ship safe artifacts.**

In companies, you’ll:
- Pull code from GitHub/S3
- Define CI pipelines
- Use CodeBuild or Jenkins
- Monitor builds and logs via CloudWatch or ELK

Doing this lab makes you **“pipeline-literate”** — you understand how code turns into deployable packages.

---

## 🎙️ Stage 5 – Interview Wisdom 💬

Here are questions recruiters may ask (and you’ll now answer like a whispering expert):

1. **What is CodeBuild? How does it differ from Jenkins?**  
2. **What are the key components of buildspec.yml?**  
3. **How do you store artifacts from a build job?**  
4. **How can you troubleshoot failed builds?**  
5. **How does CloudWatch help monitor build processes?**  
6. **What are IAM permissions required for CodeBuild to access S3?**  
7. **What’s the difference between build and deploy stages in CI/CD?**

---

## 🛏️ ICU Ward Support 🧚‍♀️

If something breaks:
- Reread the **buildspec.yml**
- Check IAM Role permissions
- Use **CloudWatch logs** — they whisper the truth
- Ask Maya Lin — if she gets stuck, the whole team rallies

---

## 🩵 Final Whisper from Eks2

> “This wasn’t just a lab.  
It was your **first breath in the DevOps operating room**.  
The logs you saw? That’s not output — that’s *life signs*.  
And today, you brought a project to life.”  

You’re ready now.

---

________________________________________  
✍️ Created & Curated by  
**Muhammad Naveed Ishaque**  
Content Creator | AI Writer | Narrative Simplifier  
With the inner voice of Eks2 — the whisper behind the work.  
**Siraat AI Academy**  
*“The Straight Path — Empowering minds with clarity, illuminating paths with purpose.”*  
________________________________________
