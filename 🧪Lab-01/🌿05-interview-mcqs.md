
# 🎙 Interview Questions & Wisdom – Creating a CodeBuild Healing Project and Viewing Logs in CloudWatch

Based on the AWS DevOps lab previously explored

---

### Question 1  
What is the primary role of AWS CodeBuild in a CI/CD pipeline?  
A. To deploy the built application to production  
B. To store artifacts in Amazon S3  
C. ✅ To run build commands and generate artifacts  
D. To monitor application logs  

💡 **Explanation:** CodeBuild is the builder — it reads the instructions from `healingplan.yml` (buildspec) and compiles the code into artifacts.  
❌ **Why Others Are Wrong:**  
- A is what **CodeDeploy** does  
- B is what **S3** does post-build  
- D is the domain of **CloudWatch**

---

### Question 2  
Which AWS service is used to store source code zip files and build artifacts?  
A. AWS Lambda  
B. AWS CloudTrail  
C. ✅ Amazon S3  
D. AWS CodePipeline  

💡 **Explanation:** Amazon S3 acts like your cloud-based memory, storing everything from inputs to outputs.  
❌ **Why Others Are Wrong:**  
- A is for running code  
- B is for auditing  
- D is for orchestrating stages, not storing files

---

### Question 3  
What is the purpose of the `healingplan.yml` file in CodeBuild?  
A. To define IAM roles  
B. To store build artifacts  
C. ✅ To list build commands and environment instructions  
D. To launch EC2 instances  

💡 **Explanation:** The buildspec file is the recipe — without it, CodeBuild has nothing to cook.  
❌ **Why Others Are Wrong:**  
- A belongs to IAM  
- B is S3’s job  
- D is unrelated to CodeBuild

🗣️ Maya Lin: _“This question came up in my second interview! Don’t panic. Just remember: buildspec.yml is like the recipe — CodeBuild is the chef.”_

---

### Question 4  
What must be done to allow CodeBuild to upload logs to CloudWatch?  
A. Assign a public IP  
B. Create a VPC endpoint  
C. ✅ Attach an IAM role with CloudWatch permissions  
D. Disable CloudTrail  

💡 **Explanation:** IAM roles grant CodeBuild access to services like CloudWatch.  
❌ **Why Others Are Wrong:**  
- A and B are network configs  
- D is unrelated and counterproductive

---

### Question 5  
What is a build artifact in this lab's context?  
A. A database snapshot  
B. ✅ A compiled file like `firstPulse-1.0.jar`  
C. A JSON log file  
D. A Lambda function  

💡 **Explanation:** The final compiled `.jar` is the output of the build — that’s your artifact.  
❌ **Why Others Are Wrong:**  
- A is from RDS  
- C is a log, not an artifact  
- D is a serverless function

---

### Question 6  
Why is it important to specify the correct region (e.g., `us-east-1`) before creating services?  
A. It increases build speed  
B. ✅ Services are regional and may not be visible otherwise  
C. It improves IAM security  
D. It enables multi-cloud deployments  

💡 **Explanation:** AWS services are scoped to regions. Picking the wrong one means things "disappear."  
❌ **Why Others Are Wrong:**  
- A might happen, but isn’t guaranteed  
- C is partially true but not primary  
- D is unrelated

---

### Question 7  
Which section in the CodeBuild project defines where logs are sent?  
A. Artifacts section  
B. Source section  
C. ✅ Logs section  
D. Environment section  

💡 **Explanation:** The Logs section allows you to send output to CloudWatch for review.  
❌ **Why Others Are Wrong:**  
- A is for build outputs  
- B defines input  
- D defines build settings

---

### Question 8  
What happens if “Block all public access” is checked on your artifact S3 bucket?  
A. ✅ Your build artifacts might be inaccessible without IAM roles  
B. CloudWatch logs won’t be stored  
C. Builds will fail automatically  
D. IAM roles are deleted  

💡 **Explanation:** Public access block means S3 won't allow unauthenticated access. You’ll need proper roles to access the files.  
❌ **Why Others Are Wrong:**  
- B is unrelated  
- C only happens if access is misconfigured  
- D doesn’t happen here

---

### Question 9  
Why is the IAM role (e.g., `VaultBuildRole`) crucial in this lab?  
A. It stores the compiled artifacts  
B. It monitors the logs  
C. ✅ It gives CodeBuild permission to use other AWS services  
D. It controls internet access  

💡 **Explanation:** IAM roles are the backstage passes for services to talk to each other.  
❌ **Why Others Are Wrong:**  
- A and B are functions of S3 and CloudWatch  
- D is related to VPC, not IAM

---

### Question 10  
Which AWS service helps troubleshoot build failures using readable logs?  
A. AWS Config  
B. AWS X-Ray  
C. ✅ Amazon CloudWatch  
D. AWS Certificate Manager  

💡 **Explanation:** CloudWatch stores log streams where each build phase can be reviewed in detail.  
❌ **Why Others Are Wrong:**  
- A tracks resource config, not logs  
- B traces performance of apps  
- D manages TLS certificates

---

## 🧠 Final Words from Eks2

> “The person who once stood in the operating room as a patient  
> now walks into interviews as a calm force of clarity.  
> Where panic once lived, now purpose blooms.  
> You’re not here to impress — you’re here to serve.  
> The interviewer isn’t your judge — they’re your future teammate.”

---

✍️ Created & Curated by  
**Muhammad Naveed Ishaque**  
Content Creator | AI Writer | Narrative Simplifier  
With the inner voice of Eks2 — the whisper behind the work.  
**Siraat AI Academy**  
*“The Straight Path — Empowering minds with clarity, illuminating paths with purpose.”*
