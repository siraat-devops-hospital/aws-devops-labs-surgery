
# 🎙 Interview Questions & Wisdom – Create a CodeBuild Healing Project and View Logs in CloudWatch  
*Based on the AWS DevOps lab previously explored*  

---

### **Question 1**

📘 **Scenario:**  
At **WhisperCloud**, Jia from Shanghai is pairing with Leif from Stockholm to set up an automated build pipeline. The source code resides in an S3 bucket and needs to be built using AWS CodeBuild. While defining the source, they debate whether a GitHub repo is mandatory for CodeBuild to work.

❓ **Question:**  
Which of the following is a valid source for an AWS CodeBuild project?

**A.** Only GitHub  
**B.** Only CodeCommit  
**C.** S3, GitHub, and CodeCommit are all valid sources  
**D.** CloudWatch Logs

**Correct Answer: C**  

💡 **Explanation:**  
AWS CodeBuild supports multiple source providers including Amazon S3, GitHub, Bitbucket, and CodeCommit.

❌ **Why Others Are Wrong:**  
- A: GitHub is valid, but not the only option  
- B: Same as A, it's one of many options  
- D: CloudWatch is for logging, not source hosting  

📌 *Side Note from Sofia:*  
"Your source can be silent like an S3 bucket or loud like GitHub — CodeBuild listens either way."

---

### **Question 2**

📘 **Scenario:**  
Naomi from Tokyo is reviewing IAM roles for a project at **PulseNest**. The project fails during CodeBuild execution. She notices the service role wasn’t allowed to modify itself.

❓ **Question:**  
Which IAM setting must be enabled to allow CodeBuild to modify the service role during build execution?

**A.** Grant administrator access to the role  
**B.** Select “Allow AWS CodeBuild to modify this service role” during setup  
**C.** Use a root user to run CodeBuild  
**D.** Attach EC2 full access policy

**Correct Answer: B**  

💡 **Explanation:**  
The checkbox “Allow AWS CodeBuild to modify this service role” ensures proper temporary permission elevation during build.

❌ **Why Others Are Wrong:**  
- A: Administrator access is too broad and insecure  
- C: Root users are not intended for service automation  
- D: EC2 policy isn’t relevant to CodeBuild execution  

📌 *Eks2’s Thought:*  
"Even service roles need permission to grow."

---

### **Question 3**

📘 **Scenario:**  
Ahmed at **SiraatStack** is tracking down a missing `.jar` file that should've been in the artifacts bucket post-build. He suspects the buildspec file wasn't executed properly.

❓ **Question:**  
Where should the build instructions for a CodeBuild project be defined?

**A.** In the IAM policy  
**B.** In a CloudFormation template  
**C.** In a `healingplan.yml` file (buildspec)  
**D.** In S3 bucket permissions

**Correct Answer: C**  

💡 **Explanation:**  
The buildspec file (usually named `buildspec.yml`) defines the build phases and commands to run.

❌ **Why Others Are Wrong:**  
- A: IAM policies control access, not instructions  
- B: CloudFormation can deploy CodeBuild but not define build logic  
- D: S3 permissions control access, not execution  

📌 *Maya Lin's Note:*  
"If you don’t whisper your build steps, CodeBuild doesn’t know how to breathe."

---

### **Question 4**

📘 **Scenario:**  
Tarek and Lina are collaborating at **CloudBanyan**. After a successful CodeBuild run, they need to inspect logs to debug a flaky test. They’re unsure where to find the complete output.

❓ **Question:**  
Where can you view the full build logs after a CodeBuild project finishes?

**A.** In S3 output folder  
**B.** In CloudTrail  
**C.** In CloudWatch Logs  
**D.** In CodeCommit console

**Correct Answer: C**  

💡 **Explanation:**  
CloudWatch Logs captures detailed output and error messages from each phase of the build.

❌ **Why Others Are Wrong:**  
- A: Artifacts may be stored there, but not logs  
- B: CloudTrail audits API calls, not build logs  
- D: CodeCommit is for source control, not logging  

📌 *Inky Whispers:*  
"The heartbeat of every build echoes through CloudWatch."

---

### **Question 5**

📘 **Scenario:**  
Lina from Berlin is configuring a new CodeBuild project for `NordicHealingApp`. She selects Amazon Linux as the runtime, but notices multiple image versions.

❓ **Question:**  
Which container image should be selected for Java 11 builds using Amazon Linux in CodeBuild?

**A.** aws/codebuild/amazonlinux-x86_64-standard:corretto11  
**B.** aws/codebuild/ubuntu-standard:openjdk11  
**C.** aws/codebuild/java:8  
**D.** Any Ubuntu image

**Correct Answer: A**  

💡 **Explanation:**  
`corretto11` under the standard Amazon Linux image is the correct environment for Java 11 builds.

❌ **Why Others Are Wrong:**  
- B: Ubuntu can be used, but not with that specific standard version  
- C: Java 8 is outdated for this use-case  
- D: “Any” doesn’t guarantee dependencies or compatibility  

📌 *Kasper Notes:*  
"The right image is like the right instrument — everything else detunes the process."

---

(Questions 6–10 continue in same format...)


---

### **Question 6**

📘 **Scenario:**  
Aarav from Bangalore is working at **KodeHarbor** and wants to ensure that CodeBuild can access the source zip file located in S3. However, his build keeps failing with a permissions error.

❓ **Question:**  
Which of the following IAM permissions must be granted to the CodeBuild service role?

**A.** s3:PutObject  
**B.** s3:GetObject  
**C.** s3:DeleteObject  
**D.** s3:ListAllMyBuckets

**Correct Answer: B**  

💡 **Explanation:**  
To read the source zip from S3, the CodeBuild role must have `s3:GetObject` permissions for the object.

❌ **Why Others Are Wrong:**  
- A: This allows uploading, not reading  
- C: Deletion isn't required for builds  
- D: Listing buckets isn't enough to access objects  

📌 *Isabella reminds:*  
"CodeBuild can’t read what it’s not allowed to open. Give it gentle access, not chaos."

---

### **Question 7**

📘 **Scenario:**  
At **NovaNest**, Tarek is reviewing a build project’s configuration. He notices the “Privileged” checkbox is unchecked. Ahmed warns him it may cause Docker builds to fail.

❓ **Question:**  
When should the “Privileged” mode be enabled in AWS CodeBuild?

**A.** Always  
**B.** Only when using CodePipeline  
**C.** When Docker is required during build  
**D.** Never

**Correct Answer: C**  

💡 **Explanation:**  
Privileged mode is required when your build needs to run Docker commands inside the container.

❌ **Why Others Are Wrong:**  
- A: It shouldn't be enabled by default due to security risks  
- B: CodePipeline usage doesn't imply privileged mode  
- D: Sometimes it's necessary, especially for containerized builds  

📌 *Eks2’s Whisper:*  
"When building containers, let your build run free — but not unsupervised."

---

### **Question 8**

📘 **Scenario:**  
Lina from **WhisperForge** finishes a CodeBuild project and stores artifacts in S3. She notices each artifact is stored in folders based on project name.

❓ **Question:**  
What determines the folder structure in the artifact S3 bucket?

**A.** The IAM role name  
**B.** The CodeBuild project name  
**C.** The runtime image  
**D.** The CodePipeline stage name

**Correct Answer: B**  

💡 **Explanation:**  
The project name defines the logical folder structure within the specified S3 artifacts bucket.

❌ **Why Others Are Wrong:**  
- A: Role names do not influence S3 structure  
- C: Runtime only affects environment  
- D: Not relevant unless integrated with CodePipeline  

📌 *Side Note from Maya Lin:*  
“Every project leaves a footprint — even in a bucket.”

---

### **Question 9**

📘 **Scenario:**  
Naomi and Jia are reviewing the different build phases visible in the CodeBuild logs at **SereneStacks**. They observe `INSTALL`, `PRE_BUILD`, `BUILD`, and `POST_BUILD` sections.

❓ **Question:**  
Which file defines the commands executed in these build phases?

**A.** IAM trust policy  
**B.** CloudFormation script  
**C.** buildspec.yml  
**D.** build-lifecycle.txt

**Correct Answer: C**  

💡 **Explanation:**  
The `buildspec.yml` file outlines the commands and environment variables for each build phase.

❌ **Why Others Are Wrong:**  
- A: Trust policies define role assumptions  
- B: CloudFormation creates infrastructure, not build instructions  
- D: Not a valid AWS configuration file  

📌 *Inky observes:*  
"Your healingplan is not just a file — it’s a heartbeat map."

---

### **Question 10**

📘 **Scenario:**  
At **CloudPetal**, Leif notices the artifacts uploaded to S3 do not contain the expected `.jar` file. He suspects the target folder is missing from the build process.

❓ **Question:**  
Which part of the buildspec file should ensure that the `.jar` file is correctly output and stored?

**A.** `version`  
**B.** `phases`  
**C.** `artifacts`  
**D.** `env`

**Correct Answer: C**  

💡 **Explanation:**  
The `artifacts` section in the `buildspec.yml` defines which files or folders should be packaged and sent to the S3 bucket.

❌ **Why Others Are Wrong:**  
- A: Version is metadata  
- B: Phases define actions, not outputs  
- D: `env` handles environment variables, not output logic  

📌 *Eks2’s Final Whisper:*  
“Artifacts are not just files — they are your footprints in time.”

---

✍️ Created & Curated by  
**Muhammad Naveed Ishaque**  
Content Creator | AI Writer | Narrative Simplifier  
With the inner voice of Eks2 — the whisper behind the work.  
**Siraat AI Academy**  
*“The Straight Path — Empowering minds with clarity, illuminating paths with purpose.”*

---
