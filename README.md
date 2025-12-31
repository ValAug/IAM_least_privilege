# 🔐 IAM Least Privilege Demo  
### Terraform + AWS Lambda + Amazon S3

This repository demonstrates **IAM Least Privilege in practice** using a **minimal, real AWS setup**.

Instead of theory, this project shows how to:
- Grant **only the permissions a workload actually needs**
- Scope access down to **specific S3 prefixes**
- Avoid common IAM over-permissioning mistakes
- Validate behavior through real Lambda execution

---

## 🧭 Why This Project Exists

IAM policies often become overly broad because they are:
- Hard to reason about
- Copied from examples
- Written “just to make it work”

This demo proves that:
> **Least privilege is achievable, understandable, and practical**  
—even for everyday serverless workloads.

---

## 🧱 What Gets Deployed

This project intentionally deploys **only what is required**:

- 🪣 **One Amazon S3 bucket**
- ⚡ **One AWS Lambda function**
- 🔐 **One IAM role with tightly scoped permissions**
- 📜 **CloudWatch Logs** for observability

No VPCs.  
No extra services.  
No hidden permissions.

---

## 🔍 Lambda Permission Model

The Lambda function operates under a **strict IAM role**.

### ✅ Allowed Actions

| Service | Permission | Scope |
|-------|------------|-------|
| S3 | Read | `s3://<bucket>/public/*` |
| S3 | Write | `s3://<bucket>/results/*` |
| CloudWatch | Write logs | Lambda log group only |

### 🚫 Explicitly Not Allowed

- ❌ List all S3 buckets
- ❌ Read or write outside approved prefixes
- ❌ Access other AWS services
- ❌ Assume additional roles

> If the Lambda tries to exceed its permissions, **AWS denies the request** — as it should.

---

## 🧠 Key Least-Privilege Principles Demonstrated

- **Prefix-level S3 access**, not bucket-wide access
- **Single-purpose IAM role**
- **No wildcard actions** (`*`)
- **No wildcard resources** beyond what’s required
- **Permissions match runtime behavior**, not convenience

---

## 🚀 Deploy the Demo

### Prerequisites

- AWS CLI configured (`aws configure`)
- Terraform installed (v1.x recommended)

---

### Deployment Steps

```bash
cd terraform
terraform init
terraform apply
````

Terraform will:

1. Create the S3 bucket
2. Create the IAM role and policy
3. Deploy the Lambda function
4. Configure logging permissions

---

## 🧪 How to Validate Least Privilege

After deployment, you can:

* Invoke the Lambda and confirm:

  * Reads from `public/` succeed
  * Writes to `results/` succeed
* Modify the Lambda code to:

  * Access a forbidden S3 path
  * Call an unauthorized AWS service

👉 You will see **AccessDenied** errors in CloudWatch Logs.

This is **expected and desired behavior**.

---

## ⚠️ Common Anti-Patterns This Project Avoids

* ❌ `AmazonS3FullAccess`
* ❌ `Resource: "*"`
* ❌ Shared IAM roles
* ❌ Permissions added “just in case”
* ❌ Manual IAM edits outside Terraform

---

## 🧩 When to Use This Pattern

This approach is ideal for:

* Serverless applications
* Data processing Lambdas
* Security reviews & audits
* IAM training and onboarding
* Proof-of-concept architectures

---

## 📚 Learn More

* IAM Best Practices
  [https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)

* AWS Lambda Permissions
  [https://docs.aws.amazon.com/lambda/latest/dg/lambda-permissions.html](https://docs.aws.amazon.com/lambda/latest/dg/lambda-permissions.html)

* Terraform AWS IAM
  [https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role)

---

✨ **Least privilege is not restrictive — it’s protective.**
This demo shows how to do it *right*, not just *fast*.
Just tell me 👍
```
