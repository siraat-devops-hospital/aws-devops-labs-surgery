
# 🛠️ Project 1: CI/CD Pipeline & CloudWatch Integration

## 📌 Project Title
**Create a CodeBridge Build Project with SkyWatch Logs Monitoring**

---

## ⚙️ Tools Used and When to Use Them

Each tool is selected like a specialist in Eks2's DevOps Hospital. Here's the complete diagnosis and surgical usage timeline:

---

### 1. **CodeBuild** *(aka CodeBridge 🛠️)*  
**Used For:** Compiling source code, running unit tests, and producing deployable artifacts.

- **When Used:**  
  During creation of the build project — it's the operating table.
- **Eks2’s Insight:**  
  “CodeBridge doesn’t just build your app. It builds your confidence.”

---

### 2. **S3 (Simple Storage Service)** *(aka VaultStore 📦)*  
**Used For:** Hosting input source zip and storing output artifacts.

- **When Used:**  
  - First: To fetch `MessageUtil.zip` for build input.  
  - Later: To store generated `.jar` files as build output.
- **IK’s Reminder:**  
  “A vault is only as strong as the care you take naming it.”

---

### 3. **CloudWatch Logs** *(aka SkyWatch 🌌)*  
**Used For:** Observing logs in real-time, catching errors and success messages.

- **When Used:**  
  After build execution, to validate every command’s echo.
- **Ayla Rune Says:**  
  “Logs are not noise — they are the stethoscope of DevOps.”

---

### 4. **IAM (Identity & Access Management)** *(aka TrustWeave 🛡️)*  
**Used For:** Creating a service role to allow CodeBuild access to S3 and CloudWatch.

- **When Used:**  
  While configuring the CodeBuild project’s permissions.
- **Isabella Konti Adds:**  
  “Never give more than needed. Trust must be earned, not assumed.”

---

### 5. **Amazon Linux Container (Corretto 11)** *(aka CleanSurgeryOS 🧼🐧)*  
**Used For:** Providing a runtime for Java-based builds.

- **When Used:**  
  When selecting the environment image in CodeBuild.
- **Elina Petrova Notes:**  
  “Use clean, predictable environments — it’s like scrubbing into surgery.”

---

### 6. **Buildspec.yml** *(aka OperationScript 📜)*  
**Used For:** Step-by-step instructions for build stages.

- **When Used:**  
  Automatically read by CodeBuild during the build process.
- **Sofia Zaymera Reminds:**  
  “Every line is a heartbeat. Write it with care.”

---

### 7. **Maven & pom.xml** *(aka PotionMaker ⚗️)*  
**Used For:** Building Java project, managing dependencies.

- **When Used:**  
  Triggered automatically during the CodeBuild run.
- **Mr. Eks2 Reflects:**  
  “Even a whisper of XML can summon powerful magic.”

---

### 8. **SNS (Optional, for Advanced Monitoring)** *(aka WhisperLine 🔔)*  
**Used For:** Sending alerts during build phase transitions (optional in this lab).

- **When Used:**  
  Configurable in CloudWatch settings.
- **Maya Lin Asks:**  
  “What if a build fails and no one’s there to hear it?”

---

## 🌸 Final Thought

This project isn’t just a set of tools — it’s a **living ecosystem** of clarity, control, and care.  
Each tool has a purpose, and together, they create a pipeline that breathes like a healthy patient on the table — one that will soon run, serve, and shine in production.

---

✍️ Created & Curated by  
**Muhammad Naveed Ishaque**  
_Content Creator | AI Writer | Narrative Simplifier_  
_With the inner voice of Eks2 — the whisper behind the work._

**Siraat AI Academy**  
_“The Straight Path — Empowering minds with clarity, illuminating paths with purpose.”_
