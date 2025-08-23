# 🌟 Project 8: Serverless Workflow with Step Functions (Fear → Flow)

## 🎯 Goal
Build a **multi-step workflow** using **AWS Step Functions** and **Lambda**: validate an input, process it, and send a notification — with **retries**, **error handling**, and clear **observability**.

> 🕊️ **Eks2 Soul Note:** Orchestration is like calm breathing for systems. Step Functions makes complexity gentle — one safe step at a time.

---

## 🗂 Repo Structure
```
aws-stepfunctions-workflow/
├─ lambdas/
│  ├─ validate/index.py
│  ├─ process/index.py
│  └─ notify/index.py
├─ state-machine/
│  └─ workflow.json
├─ policy/
│  ├─ sf-execution-role.json
│  └─ lambda-basic-role.json
└─ README.md
```

---

## 🧩 Lambda Functions (Python 3.11)

### 1) Validate (reject if missing `name`)
**lambdas/validate/index.py**
```python
import json

def lambda_handler(event, context):
    # Expecting {"name": "Alice"} at minimum
    if "name" not in event or not event["name"]:
        raise ValueError("VALIDATION_ERROR: 'name' is required")
    # Pass through; add trace field
    event["validated"] = True
    return event
```

### 2) Process (simulate some work)
**lambdas/process/index.py**
```python
import json, time, os, random

def lambda_handler(event, context):
    # Simulate non-deterministic work with a tiny chance of transient error
    if random.random() < 0.1:
        # 10% transient error to exercise Retry policy
        raise RuntimeError("TRANSIENT: downstream timeout")
    time.sleep(1)  # simulate processing
    event["processed"] = True
    event["message"] = f"Hello {event['name']} from Eks2 Step Functions 🚀"
    return event
```

### 3) Notify (log message – swap with SES/SNS in real use)
**lambdas/notify/index.py**
```python
import json, os, boto3

def lambda_handler(event, context):
    # In real projects, publish to SNS/SES/Slack, etc.
    print("[NOTIFY] ", json.dumps(event))
    return {"notified": True, "payload": event}
```

> You can replace **notify** with real integrations later: SNS, SES, EventBridge.

---

## 🔧 State Machine Definition (Amazon States Language)

**state-machine/workflow.json**
```json
{
  "Comment": "Eks2 fear-free serverless workflow",
  "StartAt": "Validate",
  "States": {
    "Validate": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:REGION:ACCOUNT_ID:function:validate-fn",
      "Catch": [
        {
          "ErrorEquals": ["States.ALL"],
          "ResultPath": "$.error",
          "Next": "ValidationFailed"
        }
      ],
      "Next": "Process"
    },
    "ValidationFailed": {
      "Type": "Fail",
      "Cause": "Validation failed"
    },
    "Process": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:REGION:ACCOUNT_ID:function:process-fn",
      "Retry": [
        {
          "ErrorEquals": ["RuntimeError", "States.TaskFailed"],
          "IntervalSeconds": 2,
          "MaxAttempts": 3,
          "BackoffRate": 2.0
        }
      ],
      "Catch": [
        {
          "ErrorEquals": ["States.ALL"],
          "ResultPath": "$.error",
          "Next": "NotifyFailure"
        }
      ],
      "Next": "NotifySuccess"
    },
    "NotifySuccess": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:REGION:ACCOUNT_ID:function:notify-fn",
      "ResultPath": "$.notify",
      "Next": "Done"
    },
    "NotifyFailure": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:REGION:ACCOUNT_ID:function:notify-fn",
      "ResultPath": "$.notify",
      "Next": "Failed"
    },
    "Done": { "Type": "Succeed" },
    "Failed": { "Type": "Fail", "Cause": "Processing failed" }
  }
}
```

Replace **REGION** and **ACCOUNT_ID**, and ensure the Lambda function names match.

---

## 🔏 IAM (Quick + Safe)

**policy/lambda-basic-role.json** (Lambda execution)
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": "*"
    }
  ]
}
```

**policy/sf-execution-role.json** (State Machine execution role)
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [ "lambda:InvokeFunction" ],
      "Resource": [
        "arn:aws:lambda:REGION:ACCOUNT_ID:function:validate-fn",
        "arn:aws:lambda:REGION:ACCOUNT_ID:function:process-fn",
        "arn:aws:lambda:REGION:ACCOUNT_ID:function:notify-fn"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": "arn:aws:logs:REGION:ACCOUNT_ID:log-group:/aws/states/*"
    }
  ]
}
```

> Use the console to create roles quickly (it suggests the service-trust and policies). For production, scope **Resource** to exact ARNs.

---

## 🏥 Five “Surgery” Stages (Step-by-Step)

### 1) Prepare (Create Lambdas)
- Create 3 Lambda functions: `validate-fn`, `process-fn`, `notify-fn` (Python 3.11).  
- Attach **lambda basic role** (logs). Upload code files above.

### 2) Create the State Machine
- Go to **Step Functions → Create state machine**.  
- Type: **Standard** (not Express, for easier debugging).  
- Definition: paste `workflow.json` (fix ARNs).  
- Logging: enable to **/aws/states/eks2-workflow**.  
- Role: create new from template or attach **sf-execution-role** JSON.

### 3) Run the Workflow
- **Start execution** with input:
```json
{ "name": "Alice" }
```
- Success path → Validate → Process → NotifySuccess → Done.  
- Try a failure by sending `{}` to see **ValidationFailed**.  
- Try retries by re-running until a transient error occurs (10% chance).

### 4) Observe (Fear → Flow)
- **Graph view** shows green path, red failures.  
- **Execution details** show inputs/outputs at each state.  
- **CloudWatch Logs** for Lambdas help you trace exact behavior.

### 5) Clean Up
- Delete the **state machine** (retain logs if you want).  
- Remove test Lambda functions/roles if demo-only.

---

## 🧯 Oxygen Mask (Troubleshooting)
- **AccessDenied (InvokeFunction)** → State machine role missing permission for one of the Lambda ARNs.  
- **Malformed definition** → Validate JSON with the ASL editor in console.  
- **Lambda import errors** → Ensure handler path = `index.lambda_handler`.  
- **Express vs Standard** → Choose **Standard** if you need the detailed visual debugger.

---

## 📄 README Skeleton
```markdown
# Step Functions: Validate → Process → Notify

## Goal
Fear-free serverless orchestration with retries and error handling.

## Steps
1) Create Lambda functions: validate-fn, process-fn, notify-fn
2) Create State Machine (Standard) using workflow.json
3) Start execution with {"name":"Alice"}
4) Observe graph & logs; test failure with {}

## Cleanup
Delete state machine and Lambdas if demo-only.
```

---

## 🎬 YouTube Flow (2–3 mins)
1. Soul intro: orchestration = calm breathing  
2. Code tour (validate/process/notify)  
3. State machine graph + execution  
4. Success + failure runs (show retries)  
5. Cleanup + integrations (SNS/SES/Slack next)

---

**Created & Curated by _Muhammad Naveed Ishaque (Eks2)_**  
_With AI as co-surgeon — turning orchestration into ease, one state at a time._
