
# 🚀 AWS EKS Setup from Ubuntu EC2 (Step-by-Step)

## 📌 Architecture Overview

```
Ubuntu EC2 (CLI tools)
        ↓
AWS CLI + eksctl + kubectl
        ↓
   AWS EKS Cluster
        ↓
 Worker Nodes (Managed EC2)
```

---

# 🖥️ Step 1: Launch Ubuntu EC2

* Launch an **Ubuntu EC2 instance** (t2.medium recommended)
* Allow ports:

  * SSH (22)
  * HTTP (80)
  * HTTPS (443)

### 🔐 Connect to EC2

```bash
ssh -i your-key.pem ubuntu@<public-ip>
```

---

# 🔄 Step 2: Update System

```bash
sudo apt update && sudo apt upgrade -y
```

---

# ☁️ Step 3: Install AWS CLI

```bash
sudo apt install unzip -y

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

### ✅ Verify

```bash
aws --version
```

---

# 🔐 Step 4: Configure AWS CLI

```bash
aws configure
```

Provide:

* AWS Access Key (from IAM)
* AWS Secret Key
* Region: `ap-south-1`
* Output: `json`

---

# ⚙️ Step 5: Install kubectl

```bash
curl -o kubectl https://amazon-eks.s3.ap-south-1.amazonaws.com/latest/2024-02-16/bin/linux/amd64/kubectl

chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

### ✅ Verify

```bash
kubectl version --client
```

---

# ⚙️ Step 6: Install eksctl

```bash
curl --silent --location \
"https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" \
| tar xz -C /tmp

sudo mv /tmp/eksctl /usr/local/bin
```

### ✅ Verify

```bash
eksctl version
```

---

# ☸️ Step 7: Create EKS Cluster

```bash
eksctl create cluster \
  --name my-cluster \
  --region ap-south-1 \
  --nodegroup-name my-nodes \
  --node-type t3.medium \
  --nodes 2 \
  --nodes-min 1 \
  --nodes-max 3 \
  --managed
```

### ⏱️ Note:

* Takes **10–15 minutes**
* Creates:

  * EKS Control Plane
  * VPC
  * Managed Node Group

---

# 🔗 Step 8: Connect to Cluster

```bash
aws eks --region ap-south-1 update-kubeconfig --name my-cluster
```

---

# 📊 Step 9: Verify Cluster

```bash
kubectl get nodes
```

You should see **2 worker nodes in Ready state** ✅

---

# 🚀 Step 10: Deploy Application (Nginx)

### Create Deployment

```bash
kubectl create deployment nginx --image=nginx
```

### Expose Service

```bash
kubectl expose deployment nginx \
  --type=LoadBalancer \
  --port=80
```

---

# 🌐 Step 11: Access Application

```bash
kubectl get svc
```

* Copy **EXTERNAL-IP**
* Open in browser → 🎉 Nginx running!

---

# ⚠️ Important Fixes (from your original)

✔ Use **t3.medium instead of t2.medium** (EKS recommendation)
✔ Added `--managed` (best practice)
✔ Corrected kubectl install path & permissions
✔ Structured commands cleanly
✔ Added verification steps

---



