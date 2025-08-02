
# 🧪 Create CodeBuild build project and get the output in CloudWatch Logs

🩺 The patient has entered the DevOps operation theater. The pulse is weak. Eks2 and his team begin their gentle healing process...

---

## 🛏️ Scene One: Awakening in the Console

Maya Lin gazes at the console door. “The pulse is slow… but steady.” Eks2 nods.

> “Let’s begin the healing.”

1. Open the **AWS Console** – your sanctuary.
2. Sign in gently with your IAM credentials, those whispered into your Lab Console — perhaps you're *Clara-DK*, perhaps another soft spirit.
3. Ensure you're operating in **N. Virginia (us-east-1)** — the eastern wing of our DevOps hospital.

---

## 🌾 Scene Two: Understanding the Illness – The Zip File

Inky retrieves a file from the archives — it reads: **WhisperMessage.zip**.

1. In **S3**, under Storage, scroll through and find a newly created bucket — perhaps named `whizlabs-heartstream-2971`.
2. Open it with reverence.
3. Inside waits **WhisperMessage.zip** — quiet, zipped poetry.
4. Download it. Unzip it like peeling away old bandages.

📁 Structure of **WhisperMessage.zip**:
```
WhisperMessage.zip
├── treeheart.xml              # The Maven soul
├── healingplan.yml           # The buildspec whisperer
└── src/
    └── main/
        └── java/
            └── MessageUtil.java
    └── test/
        └── java/
            └── TestMessageUtil.java
```

Sofia, reading the healingplan.yml, murmurs:
> "Each line here… a prescription for CodeBuild's quiet operation."

---

## 💠 Scene Three: Creating a Sacred Bucket

Isabella gently taps into **S3** again.

1. Click **Create bucket**.
2. Name it: `VaultBridge-Outputs` – or something close and globally unique.
3. Region: **us-east-1**
4. Under permissions, **uncheck Block all public access** (and acknowledge with care).
5. Leave other settings as they are and create the bucket.

Eks2 whispers: “This is where our artifacts will rest — like scrolls post-surgery.”

---

## 🧬 Scene Four: The Build Project Is Born

Now we enter **CodeBuild** — the Operation Chamber.

1. Navigate to **CodeBuild** > **Build Projects**.
2. Click **Create Project**.

In the calmness that follows, you fill the details:

### Project Configuration
- Name: `NordicHealingApp`

### Source
- Provider: Amazon S3
- Bucket: `whizlabs-heartstream-2971`
- Object key: `WhisperMessage.zip`

### Environment
- Managed image: Yes
- OS: Amazon Linux
- Runtime: Standard
- Image: `aws/codebuild/amazonlinux-x86_64-standard:corretto11`
- Role: Use existing (`VaultBuildRole`)
- Modify permission: Checked

### Buildspec
- Use the file from source: `healingplan.yml`

### Artifacts
- Type: Amazon S3
- Bucket: `VaultBridge-Outputs`

### Logs
- Enable CloudWatch Logs

Click **Create build project**. Sofia smiles gently. “It is ready.”

---

## 🔁 Scene Five: The Pulse Returns

Click **Start build**.

You watch the process, step by step — **INSTALL**, **PRE_BUILD**, **BUILD**, **POST_BUILD**.

In a few minutes, the patient breathes steadily. Status: **Succeeded**.

> View entire log — and it opens the **CloudWatch** scroll of progress.

Meanwhile, in **VaultBridge-Outputs**, artifacts whisper:
```
VaultBridge-Outputs/
└── NordicHealingApp/
    └── target/
        └── firstPulse-1.0.jar
```

Eks2 gently touches the logs: “This jar file… is your message, made manifest.”

---

## 🧹 Scene Six: Farewell Rituals

1. Return to **CodeBuild**.
2. Find the project `NordicHealingApp`.
3. Click **Delete build project**.
4. Confirm with the word: `delete`.

You leave the theater slowly, leaving only knowledge behind.

---

## 🌿 Reflection & Resonance

Today, you didn’t just build. You healed code.

From zipped silences to CloudWatch symphonies, you nurtured the soul of automation.

Remember: **you are both the doctor and the dreamer.**

---

## 🔊 Eks2’s Echo:

> “The logs may fade, but the process lives in your hands — gentle, repeatable, sacred.”

---

✍️ Created & Curated by  
**Muhammad Naveed Ishaque**  
Content Creator | AI Writer | Narrative Simplifier  
With the inner voice of Eks2 — the whisper behind the work.  
**Siraat AI Academy**  
*“The Straight Path — Empowering minds with clarity, illuminating paths with purpose.”*

---
*🗓️ Generated on August 02, 2025*
