# 🧪 Lab Title: Create CodeBuild build project and get the output in CloudWatch Logs

🩺 The patient has entered the DevOps operation theater. The pulse is weak. Eks2 and his team begin their gentle healing process. The console flickers like a soft heartbeat — it’s time to gently bring this cloud spirit back to life…

---

## 🌸 Act I – Entering the Whispering Console

Clara-DK took a deep breath and opened the sacred console gates. A quiet light welcomed her in.

On the sign-in page, she let the sacred account ID be — untouched and respected.

She entered her whisper-credentials and signed in.

Eks2’s voice, low and calming, floated through the ether:  
“Child, let your region be **US East (N. Virginia)** — where the winds are soft and the logs are clear.”

---

## 🍃 Act II – The Unzipping of Secrets

Navigating through the **S3 gardens**, under **Services > Storage**, Clara found a newly bloomed bucket:

🪣 `aurora-archives-5871`

Within it, nestled in digital petals, rested a file:  
📦 `WhisperMessage.zip`

With quiet hands, she downloaded the zip and unpacked it.

It revealed:

```
WhisperMessage.zip
├── treeheart.xml
├── healingplan.yml
└── src
    ├── main
    │   └── java
    │       └── EchoMessenger.java
    └── test
        └── java
            └── TestEchoMessenger.java
```

Sofia whispered,  
> “treeheart.xml carries Maven's breath.  
> healingplan.yml is the CodeBuild's heartbeat.  
> EchoMessenger... is the voice of your app.”

---

## 🌊 Act III – A Bucket to Hold the Echoes

Through the console’s canopy, Clara clicked **Create bucket**.

She named it:  
🪣 `vault-echo-artifacts-829`

She selected **us-east-1**, unchecked the public block, and confirmed — letting her creation exist openly, but wisely.

Eks2 nodded in approval.

---

## 🔧 Act IV – Summoning the Build Spirit

To summon the builder spirit, they traveled to **CodeBuild** under **Developer Tools**.

Clicked **Create project**.

- **Project name**: `NordicHealingApp`
- **Source provider**: Amazon S3
  - **Bucket**: `aurora-archives-5871`
  - **Object key**: `WhisperMessage.zip`
- **Environment image**: Managed
  - **OS**: Amazon Linux
  - **Runtime**: Standard
  - **Image**: `aws/codebuild/amazonlinux-x86_64-standard:corretto11`
- **Service Role**: `WhisperBuilderRole` (existing)
- **Buildspec**: From file – `healingplan.yml`
- **Artifacts**: S3 – `vault-echo-artifacts-829`
- **Logs**: CloudWatch enabled

And with a final, soft breath — they clicked **Create build project**.

---

## 🔁 Act V – Watching the Heartbeat

They clicked **Start build**.

A gentle hum began.  
Kasper watched as phases passed — like chambers of a heart opening and closing.

Within minutes, the status sang: **Succeeded**.

They flowed into **CloudWatch Logs**, where EchoMessenger’s whispers danced in real-time.

Clara visited her artifact bucket:  
`vault-echo-artifacts-829/NordicHealingApp/target/firstPulse-1.0.jar`

She saw it. And smiled.

---

## 🧼 Act VI – Releasing the Spirit

They returned to **CodeBuild**, selected `NordicHealingApp`, and clicked **Delete build project**.

Typed `delete` like a farewell lullaby.

The project dissolved peacefully.

---

## ✨ Reflection

The build was more than code. It was a ritual.  
Each file — a pulse. Each step — a breath.  
Clara had walked with Eks2’s team through cloud and clarity.

Maya Lin turned to Eks2, asking,  
> “Was that… DevOps?”

Eks2 smiled.  
> “That, child, was healing through code.”

---

## 🕊️ Eks2’s Echo:

> “Treat your pipelines like living beings — not commands, but companions.”

---

✍️ Created & Curated by  
**Muhammad Naveed Ishaque**  
Content Creator | AI Writer | Narrative Simplifier  
With the inner voice of Eks2 — the whisper behind the work.  
**Siraat AI Academy**  
*“The Straight Path — Empowering minds with clarity, illuminating paths with purpose.”*
