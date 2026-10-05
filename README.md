# GitOps repo content for arcocd-gitops

ไฟล์ในโฟลเดอร์นี้คือเนื้อหาที่ต้อง push ขึ้น repo
`https://github.com/monkub003/arcocd-gitops.git`

Argo CD "root" application (สร้างโดย Terraform) จะอ่านโฟลเดอร์ `apps/`
แล้วติดตั้ง child application แต่ละตัว (app-of-apps pattern)

## โครงสร้าง

```
apps/
├── aws-load-balancer-controller.yaml   # ALB/NLB controller (ต้องใส่ role ARN)
├── metrics-server.yaml                 # resource usage ของ pod/node
└── external-secrets.yaml               # ดึง secret จาก AWS Secrets Manager (ต้องใส่ role ARN)
```

## ก่อน push: แทนที่ <ACCOUNT_ID>

ไฟล์ LBC และ External Secrets มี placeholder `<ACCOUNT_ID>` อยู่
เอาค่าจริงมาจาก Terraform:

```bash
terraform output lbc_role_arn
terraform output external_secrets_role_arn
```

แล้วแก้ค่า `eks.amazonaws.com/role-arn:` ในไฟล์ให้เป็น ARN จริง

## push ขึ้น repo

push เฉพาะ "เนื้อใน" โฟลเดอร์นี้ (ให้ apps/ อยู่ที่ root ของ repo):

```bash
cd gitops
git init
git remote add origin https://github.com/monkub003/arcocd-gitops.git
git add .
git commit -m "add argocd add-on applications"
git branch -M main
git push -u origin main
```
