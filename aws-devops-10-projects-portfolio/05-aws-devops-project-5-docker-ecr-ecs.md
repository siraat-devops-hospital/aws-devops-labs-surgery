# 🌟 Project 5: Dockerize an App → Push to ECR → Deploy on ECS (Fargate)

## 🎯 Goal
Take a tiny web app, **containerize** it with Docker, **push** the image to **Amazon ECR**, and **run** it on **ECS (Fargate)** — end to end, no fear.

> 🕊️ **Eks2 Soul Note:** Containers look scary until you ship one. After this, your inner voice will say: *“DevOps se darna kyu? Eks2 hai na.”*

---

## ⚠️ Cost Note (Honest)
Fargate is not free-tier. Keep the **service running only during testing** (1 task, smallest CPU/RAM). Delete at the end to avoid charges. (EC2 launch type can be cheaper but has more setup.)

---

## 🗂 Repo Structure
```
aws-docker-ecr-ecs/
├─ app/
│  ├─ server.js
│  └─ package.json
├─ Dockerfile
├─ scripts/
│  ├─ ecr_push.sh
│  └─ ecs_cleanup.sh
└─ README.md
```

---

## 🧪 Minimal App (Node.js)

**app/package.json**
```json
{
  "name": "eks2-hello-ecs",
  "version": "1.0.0",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

**app/server.js**
```js
const express = require('express');
const app = express();
const port = process.env.PORT || 3000;

app.get('/', (req, res) => {
  res.send(`<h1>Hello from Eks2 on ECS 🚀</h1><p>Deployed via Docker → ECR → ECS (Fargate)</p>`);
});

app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
```

---

## 🐳 Dockerfile
```dockerfile
FROM node:18-alpine
WORKDIR /usr/src/app
COPY app/package.json ./
RUN npm install --omit=dev
COPY app/ ./
ENV PORT=3000
EXPOSE 3000
CMD ["npm", "start"]
```

---

## 🛰️ Create ECR Repo & Push (scripts/ecr_push.sh)

Replace `YOUR_AWS_ACCOUNT_ID` and `YOUR_REGION`.

```bash
#!/usr/bin/env bash
set -euo pipefail

REGION="eu-north-1"              # change if needed
ACCOUNT="YOUR_AWS_ACCOUNT_ID"    # 12-digit
REPO="eks2-hello-ecs"

# 1) Create repo if not exists
aws ecr describe-repositories --repository-names "$REPO" --region "$REGION" >/dev/null 2>&1 || aws ecr create-repository --repository-name "$REPO" --region "$REGION"

# 2) Login to ECR
aws ecr get-login-password --region "$REGION" | docker login --username AWS --password-stdin "$ACCOUNT.dkr.ecr.$REGION.amazonaws.com"

# 3) Build, tag, push
docker build -t "$REPO:latest" .
docker tag "$REPO:latest" "$ACCOUNT.dkr.ecr.$REGION.amazonaws.com/$REPO:latest"
docker push "$ACCOUNT.dkr.ecr.$REGION.amazonaws.com/$REPO:latest"

echo "✅ Pushed: $ACCOUNT.dkr.ecr.$REGION.amazonaws.com/$REPO:latest"
```

> Needs: AWS CLI, Docker, and credentials configured (`aws configure`).

Make executable:
```bash
chmod +x scripts/ecr_push.sh
```

Run it from repo root:
```bash
scripts/ecr_push.sh
```

Copy the full image URI it prints (you’ll paste into the task definition).

---

## 🧩 ECS Task Definition (Fargate, JSON)

Create a task definition named `eks2-hello-ecs-td` with this JSON. Replace **ACCOUNT**, **REGION**, and **IMAGE** with your ECR image URI.

```json
{
  "family": "eks2-hello-ecs-td",
  "networkMode": "awsvpc",
  "requiresCompatibilities": ["FARGATE"],
  "cpu": "256",
  "memory": "512",
  "executionRoleArn": "arn:aws:iam::ACCOUNT:role/ecsTaskExecutionRole",
  "taskRoleArn": "arn:aws:iam::ACCOUNT:role/ecsTaskRole",
  "containerDefinitions": [
    {
      "name": "web",
      "image": "ACCOUNT.dkr.ecr.REGION.amazonaws.com/eks2-hello-ecs:latest",
      "portMappings": [
        { "containerPort": 3000, "protocol": "tcp" }
      ],
      "essential": true,
      "logConfiguration": {
        "logDriver": "awslogs",
        "options": {
          "awslogs-group": "/ecs/eks2-hello-ecs",
          "awslogs-region": "REGION",
          "awslogs-stream-prefix": "ecs"
        }
      }
    }
  ]
}
```

### Required IAM Roles
- **ecsTaskExecutionRole** (managed policy: `AmazonECSTaskExecutionRolePolicy`) → pull image, write logs.  
- **ecsTaskRole** (empty/custom for app’s AWS access; can be omitted for this demo).

Create the log group:
```bash
aws logs create-log-group --log-group-name /ecs/eks2-hello-ecs --region eu-north-1 || true
```

Register the task definition (CLI alternative):
```bash
aws ecs register-task-definition   --cli-input-json file://ecs-task-def.json   --region eu-north-1
```

(Or use console: ECS → Task definitions → Create new.)

---

## 🌐 ECS Cluster + Service (Public without ALB — simplest demo)

1) **Create VPC prerequisites** (use **default VPC** to keep it simple).  
2) **Create an ECS cluster** (Networking only) → name: `eks2-cluster`.  
3) **Create a security group** (allow inbound **TCP 3000** from `0.0.0.0/0`).  
4) **Create a service**:  
   - Launch type: **Fargate**  
   - Task definition: `eks2-hello-ecs-td:1`  
   - Cluster: `eks2-cluster`  
   - Desired tasks: **1**  
   - **VPC**: choose default VPC, pick **public subnets**  
   - **Security group**: the one you created (port 3000)  
   - **Assign public IP**: **ENABLED** (important)  

5) After the task is **RUNNING**, open the task details → **ENI public IPv4** → open `http://PUBLIC_IP:3000`.

> 📝 **Note:** Public IP may change on task restart. For production, use an **ALB**; demo keeps it minimal.

---

## 🧯 Troubleshooting (Oxygen Mask)

- **Task stuck in PENDING** → Missing IAM execution role or wrong subnets/SG; ensure public subnets and `Assign public IP = ENABLED`.
- **Cannot connect** → Security group inbound doesn’t allow 3000; or you forgot to use the public IP and port.
- **Image pull failed (AUTH)** → ECR login/permissions issue; ensure `AmazonECSTaskExecutionRolePolicy` and the image URI is correct.
- **No logs** → Create `/ecs/eks2-hello-ecs` log group and ensure `awslogs` options match region/name.

---

## 🧹 Cleanup (Avoid Charges)
- Stop and delete the **service** (set desired tasks to 0, then delete).  
- Delete **cluster**, **log group**, and **security group**.  
- Optionally delete ECR repo/image.  
- Remove any **public IP / ENI** left behind automatically when service is deleted.

---

## 📄 README Skeleton
```markdown
# Docker → ECR → ECS (Fargate) Demo

## Goal
Containerize a small Node app, push to ECR, and run on ECS (Fargate).

## Steps
1) Build/push image: `scripts/ecr_push.sh` (set ACCOUNT/REGION)
2) Create task definition (Fargate), roles, and log group
3) Create ECS cluster + service (public IP, port 3000 allowed)
4) Open http://PUBLIC_IP:3000

## Troubleshooting
- Pending task → roles/subnets/public IP
- No response → SG inbound 3000
- Image pull → ECR policy/URI
```

---

## 🎬 YouTube Flow (3–4 mins)
1. What we’ll build (diagram)  
2. App + Dockerfile tour  
3. ECR push script (live run)  
4. ECS task & service (public IP open)  
5. Test + cleanup + ALB mention (for prod)

---

**Created & Curated by _Muhammad Naveed Ishaque (Eks2)_**  
_With AI as co-surgeon — turning containers into confidence, one ship at a time._
