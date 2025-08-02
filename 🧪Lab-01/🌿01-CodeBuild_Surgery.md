
# 🧪 Creating a CodeBuild Healing Project and Viewing Logs in CloudWatch

🩺 The patient has entered the DevOps operation theater. The pulse is weak. Eks2 and his team begin their gentle healing process...

---

## 🧬 Task 1: Enter the Healing Console

Maya Lin wipes her palms, still nervous, and Eks2 whispers:  
_"Don't worry, every healer steps into the unknown."_

1. Go to the **AWS Management Console**.
2. On the sign-in screen, enter the soft IAM credentials from the kodeclinic console.
3. Region: set to **US East (N. Virginia)** — _us-east-1_, the land where most healing begins.

---

## 🧳 Task 2: Understand the Essence Inside the Zip

Sofia unzips the package gently, as if unwrapping a soul.

1. Go to **S3** from the Services menu.
2. Open the latest bucket named something like `siraatlabs-251187`.
3. Inside, find a file named `WhisperMessage.zip`.
4. Download and gently extract it.

Structure revealed:

```
WhisperMessage.zip  
├── treeheart.xml  
├── healingplan.yml  
└── src  
    ├── main  
    │   └── java  
    │       └── MessageUtil.java  
    └── test  
        └── java  
            └── TestMessageUtil.java
```

📜 **treeheart.xml** — Maven’s soulmap.  
📝 **healingplan.yml** — The sacred instructions for CodeBuild.  
🧾 **MessageUtil.java** — The patient’s echo.  
🧪 **TestMessageUtil.java** — The soft whisper, "Hi!Clara".

---

## 🌊 Task 3: Create the Artifact Stream

Kasper steps forward to store the output of healing.

1. Go to **S3 > Create bucket**.
2. Bucket name: `VaultBridge-Outputs`
3. Region: **us-east-1**
4. Uncheck “Block all public access” and acknowledge.
5. Click **Create**.

---

## 🏗️ Task 4: Build the Healing Chamber

Eks2 leads Maya Lin into **CodeBuild**, like an attending guiding a resident.

1. Go to **CodeBuild > Build Projects > Create project**.
2. Name: `NordicHealingApp`
3. Source provider: **Amazon S3**
   - Bucket: `siraatlabs-251187`
   - Object key: `WhisperMessage.zip`
4. Environment:
   - Image: `aws/codebuild/amazonlinux-x86_64-standard:corretto11`
   - Service Role: `VaultBuildRole`
5. Buildspec: Use `healingplan.yml`
6. Artifacts:
   - Type: Amazon S3
   - Bucket: `VaultBridge-Outputs`
7. Logs:
   - Enable **CloudWatch Logs**

Click **Create build project**.

---

## 🧪 Task 5: Start the Healing

1. Click **Start build**.
2. Watch the phases – from **installing pulse** to **applying stitches**.
3. When it says **Succeeded**, go to **CloudWatch Logs** and read the healing verses.
4. Now, visit the `VaultBridge-Outputs` bucket:
   - Path: `NordicHealingApp/target/firstPulse-1.0.jar`

Inky nods silently. "This is how we measure recovery."

---

## 🧼 Task 6: Clean the Ward

Isabella reminds softly, "Every good healer also unbuilds."

1. Go to **CodeBuild > Build Projects**
2. Select `NordicHealingApp`
3. Click **Delete**
4. Confirm by typing: `delete`

---

## 🌿 Completion & Reflection

🕊️ You've entered the ward, observed the patient's code, and gently built it into life. You’ve watched it speak, “Hi!Clara,” in the logs — and smiled like a true DevOps healer.

Eks2’s Echo:

> _"Logs are just memories of actions. Heal them, understand them, and let them speak."_  
> _– Eks2, Chief Surgeon of Siraat DevOps Hospital_

---

✍️ Created & Curated by  
**Muhammad Naveed Ishaque**  
Content Creator | AI Writer | Narrative Simplifier  
With the inner voice of Eks2 — the whisper behind the work.  
**Siraat AI Academy**  
*“The Straight Path — Empowering minds with clarity, illuminating paths with purpose.”*
