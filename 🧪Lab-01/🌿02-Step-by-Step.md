
# 🩺 How to Complete It (Step-by-Step)

---

## 🧪 Step 1: Enter the Healing Console  
🔹 **Narrator:** Maya Lin  
🔹 **Voice:**  
_"Take a breath. You’re about to open a portal into the DevOps world — not with force, but with quiet trust."_  

1. Open the **AWS Console** from your kodeclinic dashboard.  
2. You’ll see a sign-in screen — think of it like washing your hands before surgery.  
   - **Username** and **Password** are waiting in your healing console. Paste them in.  
3. Don’t change the **12-digit Account ID**. It’s sacred — your access heartbeat.  
4. Set the region to **us-east-1 (N. Virginia)**.

🌿 **Soft Reminder:** Regions matter. If you choose the wrong one, your tools may not appear.  
💬 Eks2 leans in gently: _"Every region is a rhythm. Choose the one we’ve tuned for."_  

---

## 🧪 Step 2: Download the Whispered Code  
🔹 **Narrator:** Sofia  
🔹 **Voice:**  
_"Now we listen to the project. It’s zipped, but not silent. Inside, it whispers how it wants to be built."_  

1. In **S3**, find the latest bucket — it might be named `siraatlabs-251187` or similar.  
2. Inside, you’ll spot `WhisperMessage.zip`.  
3. **Click to download**. Then right-click the file and **extract it**.

Inside you'll find:
- `treeheart.xml` (like the project’s birth certificate)  
- `healingplan.yml` (a recipe for CodeBuild)  
- Java files that speak softly to each other

✨ **Tiny Victory:** You just uncovered the soul of the project.  
💬 Maya Lin whispers: _“It’s not just code… it’s intention, written down.”_

---

## 🧪 Step 3: Create a Bucket for Healing Results  
🔹 **Narrator:** Kasper  
🔹 **Voice:**  
_"Every healer needs a clean shelf to place their tools after surgery. This is your shelf."_  

1. Go to **S3 > Create bucket**.  
2. Bucket name: `VaultBridge-Outputs` (or a poetic name of your own)  
3. Region: **us-east-1**  
4. **Uncheck** “Block all public access”, then **acknowledge** the warning  
5. Click **Create bucket**

🌿 **Soft Reminder:** S3 buckets must have unique names — like every patient file.  
💬 Eks2 reflects: _“Even output needs a home.”_

---

## 🧪 Step 4: Create the Healing Project in CodeBuild  
🔹 **Narrator:** Isabella  
🔹 **Voice:**  
_"Now we prepare the chamber where healing happens — a space where code transforms."_  

1. Go to **CodeBuild > Build Projects > Create Project**  
2. Name: `NordicHealingApp`  
3. **Source**:  
   - Provider: **Amazon S3**  
   - Bucket: `siraatlabs-251187`  
   - Object Key: `WhisperMessage.zip`  
4. **Environment**:  
   - Image: `amazonlinux-x86_64-standard:corretto11`  
   - Service Role: `VaultBuildRole`  
5. **Buildspec**: Use `healingplan.yml`  
6. **Artifacts**:  
   - Type: **Amazon S3**  
   - Bucket: `VaultBridge-Outputs`  
7. **Logs**:  
   - Enable **CloudWatch Logs**

Click **Create build project**.

✨ **Tiny Victory:** You’ve prepared the surgery room.  
💬 I.K. says quietly: _“You don’t always see healing happen — but it’s there, line by line.”_

---

## 🧪 Step 5: Begin the Healing Build  
🔹 **Narrator:** Inky  
🔹 **Voice:**  
_"Now, we press the button that begins transformation. Slowly. Gently."_  

1. Select your project: `NordicHealingApp`  
2. Click **Start build**  
3. Wait as the phases unfold — **Install**, **Pre-Build**, **Build**, **Post-Build**

Once the status becomes **Succeeded**:  
- Click **View entire log** to see CloudWatch logs  
- Visit the `VaultBridge-Outputs` bucket and check for:
  - `NordicHealingApp/target/firstPulse-1.0.jar`

🌿 **Soft Reminder:** Builds take 2–5 minutes. Use that time to reflect.  
💬 Maya Lin wonders: _“Do builds feel nervous too, right before they run?”_

---

## 🧪 Step 6: Gently Let Go — Delete the Project  
🔹 **Narrator:** Eks2  
🔹 **Voice:**  
_"A good surgeon knows when to walk away. Clean, calm, and complete."_  

1. Go to **CodeBuild > Build Projects**  
2. Select `NordicHealingApp`  
3. Click **Delete project**, type `delete` to confirm  

✨ **Tiny Victory:** The chamber is empty again.  
💬 Eks2 bows gently: _“Every deletion is an act of peace — it makes room for new things.”_

---

### 📎 This step wasn’t just about a button — it was about believing that you, too, can control the cloud, one build at a time.

---

✍️ Created & Curated by  
**Muhammad Naveed Ishaque**  
Content Creator | AI Writer | Narrative Simplifier  
With the inner voice of Eks2 — the whisper behind the work.  
**Siraat AI Academy**  
*“The Straight Path — Empowering minds with clarity, illuminating paths with purpose.”*
