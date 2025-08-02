# 🌷 Lab 01 – Build & Observe with CodeBridge + SkyWatch Logs (NordicVault Labs Edition)

---

## ✨ Scenario: Mr. Eks2 Arrives at NordicVault DevOps Theater 🌤️

It was a quiet morning in **NordicVault’s SkyWing Division**, when **Mr. Eks2** stepped into the digital chamber of DevOps exploration.  
The lights were soft, the air carried the hum of pipelines waiting to be born.  
He was met by **Kasper Madsen**, joyfully sipping elderflower tea, and **Sofia Zaymera**, radiating clarity.

> “Today,” smiled **Kasper**, “you’re going to assist in a birth — a new build project using **CodeBridge**, our internal build engine.”

> “And when it breathes,” added **Sofia**, “you’ll watch its heartbeat in **SkyWatch Logs** — because every build deserves to be heard.”

---

## 🛠️ Step-by-Step Walkthrough with Eks2 & Team

### 🩺 Step 1: Entering the Console

**Eks2** gently logs into the **Aurora Console**, the gateway to NordicVault’s cloud hospital.  
He selects region `eu-north-2` (Nordhavn) — a calm and familiar space for first steps.

**Sofia** leans in, “Always choose a known ward — let your first patient breathe easy.”

---

### 📦 Step 2: Exploring the Patient File

Within the **FrystPak S3 Archive**, Eks2 finds a mysterious `.zip` file — named `WhisperCore.zip`.

**Kasper** unpacks it like a surgeon preparing instruments:  
- **buildspec.yml** – The operation script  
- **pom.xml** – The molecular blueprint  
- **HelloCloud.java**, **TestHelloCloud.java** – The spirit and the trial

**Eks2**, wide-eyed, whispers, “So the tests verify the soul?”

---

### 🧺 Step 3: Creating the Healing Vault

They open **S3Safe**, a secure storage vault used by NordicVault.

> **Sofia** explains, “This is where all output will rest — warm, safe, and retrievable.”

Eks2 names the bucket: `sky-output-whispertrail`.

---

### 🧱 Step 4: Building in CodeBridge

The team enters the **CodeBridge Panel**.  
Eks2 creates a project: `FirstWhisperBuild`.

**Kasper** configures the environment:  
- OS: Aurora Linux  
- Runtime: Corretto11  
- Role: `NVCodeRunnerRole`

Artifacts go into the earlier bucket.  
Logs are gently routed to **SkyWatch**.

**Eks2** breathes slowly. “This isn’t just a build. It’s a becoming.”

---

### 🏃‍♂️ Step 5: Initiating the Operation

He presses **Launch Build**. The console flickers.

Logs stream like morning sunlight — `INSTALL`, `PRE_BUILD`, `BUILD`, `POST_BUILD`.

**Sofia** smiles, “Every phase, a pulse.”

In **SkyWatch**, lines of green, clean, and clear — this is life.

---

### 📦 Step 6: Observing the Result

Inside the bucket:  
`sky-output-whispertrail/FirstWhisperBuild/target/hello-1.0.jar`

**Eks2** holds it digitally. “So fragile. So complete.”

---

### 🧼 Step 7: Farewell and Deletion

They thank the build.  
**Kasper** teaches Eks2 how to delete the project gently — like letting go of a patient who no longer needs the machine.

---

## 🌍 Real-World Reflection: What This Teaches

> Learning how to build with **CodeBridge** (CodeBuild) and monitor via **SkyWatch Logs** (CloudWatch)  
is the first real step for any DevOps engineer. This lab teaches not just tech — but clarity, control, and courage.

---

## 🔐 Real-World Reflection: A 158-Year Legacy Lost to One Weak Password

In 2023, a 158-year-old UK firm collapsed due to a single compromised password.  
No logs. No artifact backups. No visibility.

But in this lab, **you logged everything**, **tracked the build**, and **stored the artifacts**.  
That’s more than a task — it’s future-proofing someone’s legacy.  
🔗 [BBC News – How a Single Password Killed a Company](https://www.bbc.com/news/articles/cx2gx28815wo)

---

________________________________________  
✍️ Created & Curated by  
**Muhammad Naveed Ishaque**  
_Content Creator | AI Writer | Narrative Simplifier_  
_With the inner voice of Eks2 — the whisper behind the work._

**Siraat AI Academy**  
_“The Straight Path — Empowering minds with clarity, illuminating paths with purpose.”_  
________________________________________
