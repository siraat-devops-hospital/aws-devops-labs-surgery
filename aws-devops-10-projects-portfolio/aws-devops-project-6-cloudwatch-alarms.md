# 🌟 Project 6: CloudWatch Logs + Alarms for EC2 & Lambda → SNS Alerts

## 🎯 Goal
Set up **observability** that actually helps: stream logs, create meaningful alarms, and send alerts to **SNS (email)** — for both **EC2** and **Lambda**. No fear. Only clarity.

> 🕊️ **Eks2 Soul Note:** Monitoring is not noise. It’s care. After this surgery, your systems will **speak** — and you’ll know *exactly* when to breathe.

---

## ⚠️ Cost Note (Honest)
CloudWatch Logs/Alarms/SNS are low-cost, but **not 100% free**. Keep log retention short for demos, delete alarms/topics after testing.

---

## 🗂 Repo Structure
```
aws-cloudwatch-alarms/
├─ ec2/
│  ├─ cloudwatch-agent-config.json
│  └─ install-agent.sh
├─ lambda/
│  └─ error_demo.py
├─ alarms/
│  ├─ ec2-cpu-high.json
│  ├─ lambda-errors.json
│  └─ log-metric-filter.json
├─ sns/
│  └─ create-topic-subscribe.sh
└─ README.md
```

---

## 🧩 Part A — EC2 Logs & Metrics

### 1) Install CloudWatch Agent (Amazon Linux 2)
**ec2/install-agent.sh**
```bash
#!/usr/bin/env bash
set -euo pipefail

# Install SSM & CloudWatch agent (often SSM is preinstalled)
sudo yum update -y
sudo yum install -y amazon-cloudwatch-agent

# Put config in place
sudo mkdir -p /opt/aws/amazon-cloudwatch-agent/etc
sudo tee /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json >/dev/null <<'JSON'
{
  "agent": { "metrics_collection_interval": 60, "logfile": "/opt/aws/amazon-cloudwatch-agent/logs/agent.log" },
  "metrics": {
    "namespace": "Eks2/EC2",
    "append_dimensions": { "InstanceId": "${instance_id}" },
    "metrics_collected": {
      "cpu": { "measurement": ["cpu_usage_idle","cpu_usage_user","cpu_usage_system"], "resources": ["*"], "totalcpu": true },
      "mem": { "measurement": ["mem_used_percent"] },
      "disk": { "measurement": ["used_percent"], "resources": ["*"] },
      "net": { "measurement": ["bytes_sent","bytes_recv"], "resources": ["*"] }
    }
  },
  "logs": {
    "logs_collected": {
      "files": {
        "collect_list": [
          { "file_path": "/var/log/messages", "log_group_name": "/ec2/messages", "log_stream_name": "{instance_id}", "timestamp_format": "%b %d %H:%M:%S" },
          { "file_path": "/var/log/httpd/access_log", "log_group_name": "/ec2/httpd/access", "log_stream_name": "{instance_id}" },
          { "file_path": "/var/log/httpd/error_log", "log_group_name": "/ec2/httpd/error", "log_stream_name": "{instance_id}" }
        ]
      }
    },
    "log_stream_name": "default-stream",
    "force_flush_interval": 15
  }
}
JSON

# Start / reload agent
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl   -a stop || true

sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl   -a start   -m ec2   -c file:/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json   -s
```

> **IAM on EC2:** Attach `CloudWatchAgentServerPolicy` + `AmazonSSMManagedInstanceCore` to the EC2 instance profile to allow metrics/log shipping and SSM management.

---

### 2) EC2 Alarms (JSON → Console or CLI)

**alarms/ec2-cpu-high.json**
```json
{
  "AlarmName": "Eks2-EC2-HighCPU",
  "AlarmDescription": "CPU > 70% for 5 minutes",
  "ActionsEnabled": true,
  "MetricName": "CPUUtilization",
  "Namespace": "AWS/EC2",
  "Statistic": "Average",
  "Dimensions": [ { "Name": "InstanceId", "Value": "i-REPLACE_ME" } ],
  "Period": 60,
  "EvaluationPeriods": 5,
  "Threshold": 70.0,
  "ComparisonOperator": "GreaterThanThreshold",
  "TreatMissingData": "missing"
}
```

Create via CLI (replace region/ARNs):
```bash
aws cloudwatch put-metric-alarm   --cli-input-json file://alarms/ec2-cpu-high.json   --alarm-actions arn:aws:sns:REGION:ACCOUNT_ID:eks2-alerts   --region eu-north-1
```

> Alternative: Alarm on `StatusCheckFailed` for instance health (often more actionable).

---

## 🧩 Part B — Lambda Logs & Alarms

### 1) Lambda Logging (built-in)
**lambda/error_demo.py**
```python
import json
def lambda_handler(event, context):
    print("Hello from Eks2 — normal log line")
    raise RuntimeError("Intentional error to trigger CloudWatch Logs and Errors metric")
```

Deploy this tiny function (Python 3.11). Each invoke:
- Writes a log event to **/aws/lambda/FunctionName**
- Increments **AWS/Lambda:Errors** (namespace metric)

### 2) Alarm on Lambda Errors
**alarms/lambda-errors.json**
```json
{
  "AlarmName": "Eks2-Lambda-Errors",
  "AlarmDescription": "Lambda Errors > 0 for 3 minutes",
  "ActionsEnabled": true,
  "Namespace": "AWS/Lambda",
  "MetricName": "Errors",
  "Dimensions": [ { "Name": "FunctionName", "Value": "REPLACE_FUNCTION" } ],
  "Statistic": "Sum",
  "Period": 60,
  "EvaluationPeriods": 3,
  "Threshold": 0,
  "ComparisonOperator": "GreaterThanThreshold",
  "TreatMissingData": "notBreaching"
}
```

Create:
```bash
aws cloudwatch put-metric-alarm   --cli-input-json file://alarms/lambda-errors.json   --alarm-actions arn:aws:sns:REGION:ACCOUNT_ID:eks2-alerts   --region eu-north-1
```

> You can also alarm on **Throttles** or custom business metrics.

---

## 🧩 Part C — Log Metric Filter → Alarm (e.g., match “ERROR”)

**alarms/log-metric-filter.json** (create a metric from a log pattern)
```json
{
  "filterName": "eks2-error-lines",
  "filterPattern": "ERROR",
  "logGroupName": "/ec2/httpd/error",
  "metricTransformations": [
    {
      "metricName": "Eks2ErrorLines",
      "metricNamespace": "Eks2/Logs",
      "metricValue": "1"
    }
  ]
}
```

Create metric filter:
```bash
aws logs put-metric-filter   --cli-input-json file://alarms/log-metric-filter.json   --region eu-north-1
```

Then create an alarm on that custom metric:
```bash
aws cloudwatch put-metric-alarm   --alarm-name "Eks2-ErrorLines-Alarm"   --namespace "Eks2/Logs"   --metric-name "Eks2ErrorLines"   --statistic Sum   --period 60   --evaluation-periods 1   --threshold 1   --comparison-operator GreaterThanOrEqualToThreshold   --treat-missing-data notBreaching   --alarm-actions arn:aws:sns:REGION:ACCOUNT_ID:eks2-alerts   --region eu-north-1
```

---

## 🔔 SNS Topic + Email Subscription

**sns/create-topic-subscribe.sh**
```bash
#!/usr/bin/env bash
set -euo pipefail
REGION="eu-north-1"
EMAIL="your.email@example.com"

# Create (or get) topic
TOPIC_ARN=$(aws sns create-topic --name eks2-alerts --region "$REGION" --query 'TopicArn' --output text)

# Subscribe your email
aws sns subscribe   --topic-arn "$TOPIC_ARN"   --protocol email   --notification-endpoint "$EMAIL"   --region "$REGION"

echo "Topic: $TOPIC_ARN"
echo "👉 Check your email and CONFIRM the subscription."
```

> **Don’t forget** to click the confirmation link sent to your email — otherwise alarms can’t notify you.

---

## 🏥 Five “Surgery” Stages (Step-by-Step)

1) **Prepare** — Create SNS topic & confirm subscription. Attach IAM to EC2 instance.  
2) **EC2** — Install CloudWatch agent with provided config. See logs in `/ec2/*` groups; custom metrics in `Eks2/EC2`.  
3) **Lambda** — Deploy `error_demo.py` and test; watch **AWS/Lambda** metrics and logs.  
4) **Alarms** — Create EC2 CPU alarm, Lambda Errors alarm, and a **log metric filter** alarm for “ERROR”.  
5) **Verify** — Trigger a Lambda error or stress CPU (simple `dd` loop) and confirm **email alert** received.

Quick CPU stress (demo only, SSH):
```bash
sudo yum install -y stress || true
sudo stress --cpu 1 --timeout 180
```

---

## 🧯 Oxygen Mask (Troubleshooting)
- **No emails arriving** → SNS subscription not confirmed.  
- **No EC2 metrics** → Agent role/policy missing or agent not started. Check `/opt/aws/amazon-cloudwatch-agent/logs/agent.log`.  
- **Lambda alarm silent** → Wrong function name in alarm dimensions; wait up to 1–2 minutes for metrics.  
- **Metric filter no data** → Log group name mismatch; generate an `ERROR` line first.

---

## 🧹 Cleanup
- Delete **alarms**, **metric filter**, **log groups** (demo ones), **SNS subscription/topic**.  
- Stop/remove **EC2** if it was created for testing.

---

## 📄 README Skeleton
```markdown
# CloudWatch Logs + Alarms for EC2 & Lambda → SNS

## Goal
Stream logs and create alarms that email you when something breaks.

## Steps
1) SNS topic + email subscription
2) EC2: install CloudWatch agent (config provided)
3) Lambda: deploy error_demo.py and invoke
4) Create alarms (EC2 CPU, Lambda Errors, Log metric filter for "ERROR")
5) Trigger and verify email alert

## Cleanup
Delete alarms, metric filter, topic, test resources.
```

---

## 🎬 YouTube Flow (2–3 mins)
1. Why monitoring is care (soul note)  
2. EC2 agent + log groups appear  
3. Lambda errors + metrics  
4. Create + trigger alarms → email proof  
5. Cleanup + next steps (dashboards & Synthetics)

---

**Created & Curated by _Muhammad Naveed Ishaque (Eks2)_**  
_With AI as co-surgeon — turning noise into signal, and signal into peace._
