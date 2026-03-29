Ohhh nice 🔥 — you’ve already built a clean manual setup flow. Turning this into **Terraform** is exactly the right next DevOps move 💪

Short answer: **YES — you can (and should) do this with Terraform**.

---

# 🚀 Terraform Version of AWS EKS Setup

Instead of manually running `eksctl`, Terraform will:

✅ Create VPC
✅ Create EKS cluster
✅ Create Node Group
✅ Configure everything automatically

---

# 🧱 Step 1: Install Terraform

```bash
sudo apt install -y gnupg software-properties-common curl

curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -

sudo apt-add-repository \
"deb https://apt.releases.hashicorp.com $(lsb_release -cs) main"

sudo apt update && sudo apt install terraform -y
```

### ✅ Verify

```bash
terraform -version
```

---

# 📁 Step 2: Project Structure

```
eks-terraform/
 ├── main.tf
 ├── variables.tf
 ├── outputs.tf
```

---

# ⚙️ Step 3: Terraform Configuration

## 🧩 main.tf

```hcl
provider "aws" {
  region = "ap-south-1"
}

module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "5.1.0"

  name = "eks-vpc"
  cidr = "10.0.0.0/16"

  azs             = ["ap-south-1a", "ap-south-1b"]
  public_subnets  = ["10.0.1.0/24", "10.0.2.0/24"]
  private_subnets = ["10.0.3.0/24", "10.0.4.0/24"]

  enable_nat_gateway = true
  single_nat_gateway = true
}

module "eks" {
  source  = "terraform-aws-modules/eks/aws"
  version = "20.0.0"

  cluster_name    = "my-cluster"
  cluster_version = "1.29"

  subnet_ids = module.vpc.private_subnets
  vpc_id     = module.vpc.vpc_id

  eks_managed_node_groups = {
    default = {
      desired_size = 2
      max_size     = 3
      min_size     = 1

      instance_types = ["t3.medium"]
    }
  }
}
```

---

## 📦 variables.tf (optional basic)

```hcl
variable "region" {
  default = "ap-south-1"
}
```

---

## 📤 outputs.tf

```hcl
output "cluster_name" {
  value = module.eks.cluster_name
}
```

---

# ▶️ Step 4: Run Terraform

```bash
terraform init
terraform plan
terraform apply
```

👉 Type `yes` when prompted

⏱️ Takes ~10–15 minutes (same as eksctl)

---

# 🔗 Step 5: Connect kubectl

```bash
aws eks update-kubeconfig --region ap-south-1 --name my-cluster
```

---

# 📊 Step 6: Verify

```bash
kubectl get nodes
```

---

# 🚀 Step 7: Deploy Nginx (Same as before)

```bash
kubectl create deployment nginx --image=nginx
kubectl expose deployment nginx --type=LoadBalancer --port=80
```

---

# 💡 Why Terraform is Better (Real DevOps Talk)

| Feature          | eksctl    | Terraform  |
| ---------------- | --------- | ---------- |
| Speed            | ✅ Fast    | ✅ Fast     |
| Reusability      | ❌ Limited | ✅ High     |
| Version Control  | ❌ No      | ✅ Yes      |
| Automation       | ❌ Manual  | ✅ Full IaC |
| Production Ready | ⚠️ Medium | 🚀 High    |

---

# ⚠️ Important Notes

👉 Make sure IAM user has permissions:

* `AmazonEKSClusterPolicy`
* `AmazonEC2FullAccess`
* `IAMFullAccess` (or scoped roles)

👉 Always clean up to avoid cost:

```bash
terraform destroy
```
