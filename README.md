# 🚀 Infrastructure Drift Detection & Auto Remediation using Terraform & AWS Lambda

## 📌 Project Objective
This project demonstrates how to detect and automatically remediate infrastructure drift in AWS environments using Terraform (Infrastructure as Code) and AWS Lambda (Serverless Automation).

---

## ❗ Problem Statement
In many organizations, infrastructure is managed using Terraform. However, engineers sometimes make manual changes via the AWS Console, causing configuration drift.

This project ensures:
- Drift Detection  
- Automatic Correction  
- Continuous Compliance  

---

## 🛠️ Technologies Used
- Terraform  
- AWS Lambda  
- Amazon EventBridge  
- Amazon CloudWatch  
- AWS EC2  
- AWS S3  
- AWS IAM  

---

## 🏗️ Architecture

Terraform → AWS Infrastructure  
        ↓  
Manual Changes (Drift)  
        ↓  
EventBridge (Scheduled Trigger)  
        ↓  
Lambda Function  
        ↓  
Auto Remediation  
        ↓  
CloudWatch Logs  

---

## 📂 Project Structure

drift-detection-project/  
├── main.tf  
├── variables.tf  
├── outputs.tf  
├── lambda/  
│   └── lambda_function.py  
└── screenshots/  

---

## ⚙️ Implementation Steps

### 🔹 Step 1: Provision Infrastructure using Terraform

```bash
terraform init
terraform plan
terraform apply -auto-approve
```
Screenshots:

terraform plan

<img width="850" height="506" alt="image" src="https://github.com/user-attachments/assets/738a7e4f-7f9d-4203-9f1e-d5eedeb7ff1e" />


------


terraform apply

<img width="854" height="518" alt="image" src="https://github.com/user-attachments/assets/6e5d3f5e-5f86-42ea-854a-c399ede34dbe" />








---

### 🔹 Step 2: Infrastructure Created

Resources created:
- EC2 Instance  
- Security Group  
- S3 Bucket  

---

### 🔹 Step 3: Simulate Drift

Manual changes performed:
- Added Port 80 in Security Group  
- Changed Security Group Name  

---

### 🔹 Step 4: Detect Drift using Terraform

```bash
terraform plan
```

Terraform detects configuration drift.

---

### 🔹 Step 5: Configure Auto Remediation

- Created Lambda function  
- Scheduled using EventBridge:

```bash
rate(5 minutes)
```

---

### 🔹 Step 6: Auto Remediation

Lambda automatically fixes:
- Removes unauthorized port 80  
- Restores EC2 tag  

---

### 🔹 Step 7: Logging

All logs are stored in Amazon CloudWatch.

---

## 🧠 Lambda Function Code

```python
import boto3

ec2 = boto3.client('ec2')

def lambda_handler(event, context):
    instance_id = "REPLACE_INSTANCE_ID"
    sg_id = "REPLACE_SG_ID"

    # Restore EC2 tag
    ec2.create_tags(
        Resources=[instance_id],
        Tags=[{'Key': 'Env', 'Value': 'Dev'}]
    )

    # Remove unwanted port 80 rule
    ec2.revoke_security_group_ingress(
        GroupId=sg_id,
        IpPermissions=[{
            'IpProtocol': 'tcp',
            'FromPort': 80,
            'ToPort': 80,
            'IpRanges': [{'CidrIp': '0.0.0.0/0'}]
        }]
    )

    print("Drift Remediated Successfully")
```

---

## 🔐 IAM Policy for Lambda

```json
{
  "Effect": "Allow",
  "Action": [
    "ec2:CreateTags",
    "ec2:DeleteTags",
    "ec2:DescribeInstances",
    "ec2:DescribeSecurityGroups",
    "ec2:RevokeSecurityGroupIngress"
  ],
  "Resource": "*"
}
```

---

## 🔄 Workflow

Terraform Apply → Infrastructure Created  
↓  
Manual Changes (Drift)  
↓  
Terraform Plan detects drift  
↓  
EventBridge triggers Lambda  
↓  
Lambda fixes drift  
↓  
Logs stored in CloudWatch  

---

## 📊 Results
- Drift successfully detected  
- Automatic remediation implemented  
- Logs verified in CloudWatch  
- No manual intervention required  

---

## 🔮 Future Improvements
- Integrate AWS Config for real-time drift detection  
- Use Terraform Remote Backend (S3 + DynamoDB)  
- Automate Terraform execution via Lambda  
- Add multi-region drift detection  

---

## 🏁 Conclusion
This project ensures infrastructure consistency and security by automatically detecting and fixing configuration drift using AWS services and Terraform.

---

## 👨‍💻 Author
Bhumika Mahale
