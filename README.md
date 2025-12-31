# Version en 🇪🇸 - ⬇️

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

---
# 🇪🇸
# 🔐 Demostración de IAM Least Privilege  
### Terraform + AWS Lambda + Amazon S3

Este repositorio demuestra **IAM Least Privilege en la práctica** utilizando una **arquitectura AWS mínima y real**.

En lugar de teoría, este proyecto muestra cómo:
- Otorgar **solo los permisos estrictamente necesarios**
- Limitar el acceso a **prefijos específicos de S3**
- Evitar errores comunes de sobre-permisos en IAM
- Validar el comportamiento mediante la ejecución real de una función Lambda

---

## 🧭 ¿Por qué existe este proyecto?

Las políticas de IAM suelen volverse demasiado amplias porque:
- Son difíciles de razonar
- Se copian de ejemplos genéricos
- Se escriben “solo para que funcione”

Este demo prueba que:
> **El principio de mínimo privilegio es alcanzable, entendible y práctico**  
— incluso para cargas de trabajo serverless comunes.

---

## 🧱 ¿Qué se despliega?

Este proyecto despliega **únicamente lo necesario**:

- 🪣 **Un bucket de Amazon S3**
- ⚡ **Una función AWS Lambda**
- 🔐 **Un rol IAM con permisos estrictamente limitados**
- 📜 **CloudWatch Logs** para observabilidad

Sin VPCs.  
Sin servicios adicionales.  
Sin permisos ocultos.

---

## 🔍 Modelo de Permisos de la Lambda

La función Lambda se ejecuta con un **rol IAM muy específico**.

### ✅ Acciones Permitidas

| Servicio | Permiso | Alcance |
|--------|----------|---------|
| S3 | Lectura | `s3://<bucket>/public/*` |
| S3 | Escritura | `s3://<bucket>/results/*` |
| CloudWatch | Escritura de logs | Solo su log group |

### 🚫 Acciones NO Permitidas

- ❌ Listar todos los buckets de S3
- ❌ Leer o escribir fuera de los prefijos permitidos
- ❌ Acceder a otros servicios de AWS
- ❌ Asumir roles adicionales

> Si la Lambda intenta hacer algo fuera de sus permisos,  
> **AWS rechazará la acción automáticamente**, como debe ser.

---

## 🧠 Principios de Least Privilege Demostrados

- **Acceso a nivel de prefijo**, no a todo el bucket
- **Rol IAM con un solo propósito**
- **Sin acciones comodín (`*`)**
- **Sin recursos comodín innecesarios**
- **Permisos alineados con el comportamiento real en ejecución**

---

## 🚀 Desplegar el Demo

### Requisitos Previos

- AWS CLI configurado (`aws configure`)
- Terraform instalado (se recomienda v1.x)

---

### Pasos de Despliegue

```bash
cd terraform
terraform init
terraform apply
````

Terraform se encargará de:

1. Crear el bucket S3
2. Crear el rol y la política IAM
3. Desplegar la función Lambda
4. Configurar permisos de logging

---

## 🧪 ¿Cómo Validar el Mínimo Privilegio?

Después del despliegue puedes:

* Invocar la Lambda y confirmar que:

  * La lectura desde `public/` funciona
  * La escritura en `results/` funciona
* Modificar el código de la Lambda para:

  * Acceder a un prefijo no permitido
  * Llamar a un servicio de AWS no autorizado

👉 Verás errores **AccessDenied** en CloudWatch Logs.

Este comportamiento es **esperado y correcto**.

---

## ⚠️ Anti-Patrones que Este Proyecto Evita

* ❌ `AmazonS3FullAccess`
* ❌ `Resource: "*"`
* ❌ Roles IAM compartidos
* ❌ Permisos agregados “por si acaso”
* ❌ Cambios manuales en IAM fuera de Terraform

---

## 🧩 ¿Cuándo Usar Este Patrón?

Este enfoque es ideal para:

* Aplicaciones serverless
* Lambdas de procesamiento de datos
* Revisiones de seguridad y auditorías
* Capacitación y onboarding en IAM
* Pruebas de concepto

---

## 📚 Recursos Adicionales

* Buenas Prácticas de IAM
  [https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)

* Permisos en AWS Lambda
  [https://docs.aws.amazon.com/lambda/latest/dg/lambda-permissions.html](https://docs.aws.amazon.com/lambda/latest/dg/lambda-permissions.html)

* IAM en Terraform (AWS Provider)
  [https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role)

---

✨ **El mínimo privilegio no restringe — protege.**
Este demo muestra cómo hacerlo **bien**, no solo rápido.

---
