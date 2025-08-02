# 🌷 Lab 01 – Create a CodeBuild Project & View Logs in CloudWatch

---

## ✨ Scenario: Mr. Eks2 Enters the Lab 🌤️

As the morning mist lifts over the virtual corridor of **Siraat DevOps Hospital**,  
**Mr. Eks2** steps gently into the training theatre — a soft curiosity glimmering in his eyes.  
He’s welcomed by **Sofia Zaymera**, ever-graceful, holding a clipboard, and **Kasper Madsen**, sipping his cinnamon coffee with a grin.

> “Today,” says **Kasper**, “you’ll learn how to build something from scratch — and then *watch it breathe.*”

> “We’re going to use **AWS CodeBuild** and view the heartbeat in **CloudWatch Logs**,” adds **Sofia**.

> **Eks2** nods, “So this is where the code learns to walk?”

They all smile.

---

## 🛠️ Step-by-Step Journey Through the Healing Lab

### 🩺 Step 1: Signing into the Console

**Kasper** points to the screen. “Let’s enter the hospital. Open your AWS Console and choose the **us-east-1** region — it’s like choosing the calmest room in the hospital.”

**Sofia** reminds gently, “Never change the Account ID — this room is yours alone.”

**Eks2**, thoughtful as always, asks, “Why us-east-1?”  
**Kasper** chuckles, “Because that’s where our tools are sterilized and ready.”

---

### 📦 Step 2: Exploring the Zip File of the Patient

They walk down to the **S3 storage wing**.  
Inside a room labeled **“whizlabs...”**, they find a patient: `MessageUtil.zip`.

> **Sofia** lays the files on a table:  
> - **pom.xml** (the build's DNA)  
> - **buildspec.yml** (doctor's instructions)  
> - Source code (`MessageUtil.java`)  
> - Test file (`TestMessageUtil.java`)

**Eks2** tilts his head, “So this buildspec file… tells the CodeBuild doctor what to do?”

> “Exactly,” smiles **Kasper**, “It’s like a recipe — for healing.”

---

### 🧺 Step 3: Creating an Artifact Bucket

Now they move to the **artifact archive**.

> “We need a place to save the healing results,” says **Sofia**.  
> “Create a new **S3 bucket**, name it with care, and allow access — gently, responsibly.”

**Eks2** types: `codebuild-whiz-output-eks2`.  
A perfect home for a future miracle.

---

### 🧱 Step 4: Building the Project

They walk into the **CodeBuild operating theatre**.

> “Time to create the project,” says **Kasper**.  
> “Use **Amazon Linux** and **Corretto11** — they’re like healthy lungs and heart for the build.”

**Sofia** sets the source as the S3 file, selects the artifact bucket, and enables CloudWatch logs.  
The console glows.

**Eks2** whispers, “It feels like setting up life support.”

---

### 🏃‍♂️ Step 5: Start the Operation

The team gathers.

**Kasper** clicks **Start Build**.  
They watch the phases complete: `INSTALL`, `PRE_BUILD`, `BUILD`, `POST_BUILD`.

Logs stream like lifelines into **CloudWatch**.

> “Every green line is a breath,” says **Sofia**.  
> “You just helped something come alive.”

---

### 📦 Step 6: Check the Artifacts

They return to the bucket.  
Inside, a file awaits: `messageUtil-1.0.jar`.

**Eks2** picks it up carefully. “This is the result?”

> “Yes,” nods **Sofia**, “a heartbeat, encoded in Java.”

---

### 🧼 Step 7: Discharge the Patient

“Now,” says **Kasper**, “delete the CodeBuild project. Let this one rest. You’ve done well.”

**Eks2** sighs with quiet joy. His first DevOps procedure.

---

## 🌍 Real-World Reflection: A Healing Skillset

> Setting up **CodeBuild** and managing **CloudWatch Logs** is what real DevOps engineers do daily.  
Whether it’s building microservices, compiling packages, or testing on the fly — this is the rhythm of production.  
Today, you’ve joined that heartbeat.

---

## 🔐 Real-World Reflection: A 158-Year Legacy Lost to One Weak Password

In 2023, a **158-year-old UK firm collapsed** — all because of a **single compromised password**.  
There were no logs. No alerts. No build validations.  
Today’s lab — where you built, logged, and verified output — is your first line of defense.  
By mastering logs and artifacts, you prevent shadows from entering.  
Read the full story here: [BBC – Password Breach Collapse](https://www.bbc.com/news/articles/cx2gx28815wo)

---

________________________________________  
✍️ Created & Curated by  
**Muhammad Naveed Ishaque**  
_Content Creator | AI Writer | Narrative Simplifier_  
_With the inner voice of Eks2 — the whisper behind the work._

**Siraat AI Academy**  
_“The Straight Path — Empowering minds with clarity, illuminating paths with purpose.”_  
________________________________________
