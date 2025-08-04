
# 🛠️ proj-01-tools-and-how-they-work.md
## 🌟 Project 1: CI/CD Pipeline using CodePipeline + CodeBuild + CodeDeploy

🧪 **This is not just a pipeline — it’s a heartbeat monitor for a growing business.**  
Welcome to the DevOps Hospital’s Operation Theater for Project 1.  
Here, we introduce you to the tools that make automation magical — and you’ll befriend each one like a surgeon trusts their scalpel.

---

## 🧰 Tools Used & Their Roles in the Project

### 1️⃣ **AWS CodePipeline**
🔹 **Purpose:** Acts as the central orchestrator of the pipeline  
🔹 **How It Works:**  
- Starts on every GitHub push  
- Connects all stages: source ➝ build ➝ deploy  
🔹 **Access Path:**  
- AWS Console → Developer Tools → CodePipeline  
🔹 **In This Project:**  
- Detects GitHub changes and begins the flow automatically

---

### 2️⃣ **AWS CodeBuild**
🔹 **Purpose:** Builds your application — compiles code, runs tests, prepares artifacts  
🔹 **How It Works:**  
- Uses a `buildspec.yml` file to define build steps  
- Pulls source from CodePipeline, runs in secure containers  
🔹 **Access Path:**  
- AWS Console → Developer Tools → CodeBuild  
🔹 **In This Project:**  
- Executes the test/build of the Hello World app using Node.js or Python

---

### 3️⃣ **AWS CodeDeploy**
🔹 **Purpose:** Deploys your app to EC2 or other targets, in controlled phases  
🔹 **How It Works:**  
- Uses `appspec.yml` to control the deploy process  
- Connects with EC2 instance via IAM Role + Agent  
🔹 **Access Path:**  
- AWS Console → Developer Tools → CodeDeploy  
🔹 **In This Project:**  
- Delivers the updated build to your EC2 with no downtime

---

### 4️⃣ **GitHub (External Source Repository)**
🔹 **Purpose:** Code source and version control  
🔹 **How It Works:**  
- Connects via OAuth or PAT (Personal Access Token) to CodePipeline  
- Triggers on every push to main/dev branches  
🔹 **Access Path:**  
- https://github.com  
🔹 **In This Project:**  
- Hosts the app source code + YAML files (buildspec, appspec)

---

### 5️⃣ **Amazon EC2**
🔹 **Purpose:** Your app's target server (like lungs to breathe life into code)  
🔹 **How It Works:**  
- EC2 Agent runs and listens for CodeDeploy instructions  
- Deployed app goes live instantly after execution  
🔹 **Access Path:**  
- AWS Console → EC2 → Instances  
🔹 **In This Project:**  
- Receives and hosts your updated application

---

## 🎯 Final Deliverables

✔️ A fully functional CI/CD pipeline  
✔️ GitHub-connected CodePipeline  
✔️ Build + Deploy stages that auto-run  
✔️ EC2 with deployed Hello World app  
✔️ YouTube video explaining each tool  
✔️ GitHub repo with all source + YAML files

---

## 🌼 Why This Matters for a Layman

This is the **“DevOps Hello World”** — your first chance to  
💡 automate something,  
🧠 see it work hands-free, and  
💥 realize that DevOps is not hard — just humanized.

You are not building code.  
You are building **confidence.**

---

### ✒️ Closing Signature:

✍️ Created & Curated by  
**Muhammad Naveed Ishaque**  
_Content Creator | AI Writer | Narrative Simplifier_  
_With the inner voice of Eks2 — the whisper behind the work._

**Siraat AI Academy**  
_“The Straight Path — Empowering minds with clarity, illuminating paths with purpose.”_
