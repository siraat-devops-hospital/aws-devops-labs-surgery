# 🧪 Lab Title: Create CodeBuild build project and get the output in CloudWatch Logs

🩺 The patient has entered the DevOps operation theater. The pulse is weak. Eks2 and his team begin their gentle healing process. The console hums softly — it is time to awaken the cloud spirit...

---

## 🌸 Act I – Entering the Sacred Console

Maya Lin stood in awe, her fingers trembling slightly as she clicked **Open Console**. The sacred gates of AWS opened in a new tab.

Isabella reminded gently, “Do not touch the 12-digit ID, child. Let it be as it is, for that is the code of your presence.”

She entered her credentials — Clara-DK’s name and the hush of a password — then clicked **Sign in**. A warmth bloomed.

Eks2 whispered, “Let us remain in the eastern winds… choose **US East (N. Virginia)**, us-east-1.”

---

## 🍃 Act II – WhisperMessage.zip and Its Secrets

With steady hearts, they traveled to **S3** — under **Services > Storage**.

They sought a bucket born not long ago, a name that began as a whisper: `whizlabs-894274`.

Inside, resting quietly, lay **WhisperMessage.zip**.

Kasper helped Maya download it. “Treat it kindly,” he said, “for this archive contains the voice of our build.”

She extracted it. And like petals opening in morning light, the structure revealed:

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

Sofia leaned close. “treeheart.xml is Maven’s soul. healingplan.yml is our step-by-step chant. And MessageUtil is the messenger.”

---

## 🌊 Act III – A Bucket for Artifacts

In **S3**, they clicked **Create bucket**.

Eks2 asked, “What shall we call this healing vessel?”

Maya answered, “Let it be **VaultBridge-Outputs**.”

They selected **us-east-1**, unblocked public access, and let the bucket breathe freely.

And it was born.

---

## 🔧 Act IV – The Build Project Awakens

They went to **CodeBuild**: **Services > Developer Tools > CodeBuild > Build projects**.

Clicked **Create project**.

- **Name**: `NordicHealingApp`
- **Source provider**: Amazon S3
  - **Bucket**: `whizlabs-894274`
  - **Object key**: `WhisperMessage.zip`
- **Environment image**: Managed
  - **OS**: Amazon Linux
  - **Runtime**: Standard
  - **Image**: `aws/codebuild/amazonlinux-x86_64-standard:corretto11`
- **Service Role**: Existing — they chose `VaultBuildRole`
- **Buildspec**: From file – `healingplan.yml`
- **Artifacts**: S3 – `VaultBridge-Outputs`
- **Logs**: CloudWatch enabled

With everything in harmony, they clicked **Create build project**.

---

## 🔁 Act V – The Pulse of the Build

They pressed **Start build**.

Seconds turned to minutes. Time slowed, but the logs flowed — each stage a heartbeat.

The build whispered, then sang, then declared: **Succeeded**.

In CloudWatch, they saw the logs: glowing symbols from the digital void.

And in **VaultBridge-Outputs/NordicHealingApp/target/** rested the elixir: **firstPulse-1.0.jar**.

---

## 🧼 Act VI – Healing Concluded

They cleaned with care.

Returned to **CodeBuild > Build projects**, selected **NordicHealingApp**, clicked **Delete**.

Typed `delete` — and the build spirit returned to rest.

---

## ✨ Reflection

Eks2 gathered his team in the hospital garden.

“Remember,” he said, “we built not just code, but confidence. We listened to the whisper in the logs, followed the heartbeat of healingplan.yml, and learned that even the cloud has a soul.”

Isabella placed her hand over Maya’s.

“You walked through the winds, child. You are one of us now.”

---

## 🕊️ Eks2’s Echo:

> “Every log is a heartbeat. Every artifact, a breath. In CodeBuild’s silence… you will hear your own strength.”

---

✍️ Created & Curated by  
**Muhammad Naveed Ishaque**  
Content Creator | AI Writer | Narrative Simplifier  
With the inner voice of Eks2 — the whisper behind the work.  
**Siraat AI Academy**  
*“The Straight Path — Empowering minds with clarity, illuminating paths with purpose.”*
