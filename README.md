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

# terraform plan

<img width="850" height="506" alt="image" src="https://github.com/user-attachments/assets/738a7e4f-7f9d-4203-9f1e-d5eedeb7ff1e" />


------


# terraform apply

<img width="854" height="518" alt="image" src="https://github.com/user-attachments/assets/6e5d3f5e-5f86-42ea-854a-c399ede34dbe" />








---

### 🔹 Step 2: Infrastructure Created

Resources created:
- EC2 Instance 
- Security Group  
- S3 Bucket  

---
Screenshots:

# EC2 Instance

<img width="855" height="385" alt="image" src="https://github.com/user-attachments/assets/50fdf5fc-1e96-4b36-8225-ed86bd4899f8" />


----
# Security Group

<img width="856" height="363" alt="image" src="https://github.com/user-attachments/assets/1109cf28-1352-418f-87b6-c65fddf0cdfd" />


----
# S3 Bucket

<img width="841" height="367" alt="image" src="https://github.com/user-attachments/assets/b7d70790-b37a-47c5-9f15-4e073aada715" />



### 🔹 Step 3: Simulate Drift

Manual changes performed:
- Added Port 80 in Security Group  
- Changed Security Group Name  

---

# Port Added

<img width="848" height="361" alt="image" src="https://github.com/user-attachments/assets/a40b8743-b20e-4850-9059-a6c6a384c03c" />


# SG Name Changed

<img width="859" height="367" alt="image" src="https://github.com/user-attachments/assets/b154b25d-9fc7-4b8c-a0cc-2ece4b755108" />



### 🔹 Step 4: Detect Drift using Terraform

```bash
terraform plan
```

Terraform detects configuration drift.

Screenshot:

# Drift Detection

<img width="853" height="473" alt="image" src="https://github.com/user-attachments/assets/2497a592-eb8b-4f6a-9abf-46eb05b987f3" />

---

### 🔹 Step 5: Configure Auto Remediation

- Created Lambda function  
- Scheduled using EventBridge:

```bash
rate(5 minutes)
```
Screenshot:

# Lambda Test

<img width="843" height="365" alt="image" src="https://github.com/user-attachments/assets/700a06eb-d010-4391-9c30-b6a682e3caf5" />


---

### 🔹 Step 6: Auto Remediation

Lambda automatically fixes:
- Removes unauthorized port 80  
- Restores EC2 tag  

---

Screenshot:

# SG Fixed

<img width="844" height="366" alt="image" src="https://github.com/user-attachments/assets/b2da895b-4352-4240-9f49-dfbafb46868a" />



### 🔹 Step 7: Logging

All logs are stored in Amazon CloudWatch.

---

Screenshot:

# CloudWatch Logs

<img width="849" height="415" alt="image" src="https://github.com/user-attachments/assets/b27daf80-763f-4269-89c3-925049808712" />


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
