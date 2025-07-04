﻿
---

# 🧠 Smart Synthetic Data Generator  
**GenAI Hackathon 2025 | Impetus & AWS**

A web-based application that generates privacy-compliant synthetic datasets using **Amazon Bedrock**, designed to support AI model training in **healthcare**, **finance**, **retail**, and more.

---

## 🔍 Overview

The **Smart Synthetic Data Generator** addresses the dual challenge of **data scarcity** and **privacy concerns** in AI development. With an intuitive interface built using **Streamlit**, users can define custom dataset parameters. The system then leverages **Amazon Bedrock** to generate high-quality synthetic data that mimics real-world patterns — securely stored in **Amazon S3** and delivered via **pre-signed download links**.

---

## 🌟 Key Features

- ✅ **Streamlit UI**: Specify number of rows, column types (e.g., integer, string).
- 🤖 **Generative AI**: Uses Amazon Bedrock to generate realistic synthetic data.
- 🔒 **Secure Delivery**: CSV stored in Amazon S3 and accessed via pre-signed URL.
- ⚡ **Scalable Design**: Serverless AWS architecture using Lambda and API Gateway.
- 🌐 **Domain-Agnostic**: Supports healthcare, finance, retail, and beyond.

---

## 🧱 Tech Stack

| Component | Technology |
|-----------|------------|
| Frontend  | Streamlit (Python) |
| Backend   | AWS Lambda, API Gateway |
| AI Engine | Amazon Bedrock |
| Storage   | Amazon S3 |
| Hosting   | Amazon EC2 (for Streamlit) |
| Libraries | pandas, numpy, boto3 |

---

## 🏗️ Architecture

![diagram-export-6-17-2025-10_52_07-PM](https://github.com/user-attachments/assets/6f887ae6-0134-45a8-b772-9eaba9fe3399)


### 🔄 Components & Flow

```plaintext
User (Browser)
   ↓
Streamlit App (EC2)
   ↓ HTTP POST
API Gateway
   ↓
AWS Lambda
   ↳ Amazon Bedrock → Generate data
   ↳ pandas → Create CSV
   ↳ boto3 → Upload to S3
   ↳ S3 → Generate pre-signed URL
   ↑
Streamlit App ← API Response (URL)
   ↓
User downloads CSV
````

---

## ⚙️ Setup & Deployment

### 📋 Prerequisites

* AWS account with access to EC2, Lambda, API Gateway, S3, Bedrock
* Python 3.8+
* Git & AWS CLI configured

---

### 1. Clone Repository

```bash
git clone https://github.com/your-username/SmartSynthDataGen.git
cd SmartSynthDataGen
```

---

### 2. Deploy Streamlit on EC2

1. Launch **Amazon EC2 instance** (t2.micro, Amazon Linux 2).
2. SSH into instance:

```bash
sudo yum update -y
sudo pip3 install streamlit pandas numpy boto3
```

3. Copy `app.py` to EC2 and run:

```bash
streamlit run app.py --server.port 80
```

4. Open port `80` in your EC2 security group.
5. Visit the app at: `http://<EC2-PUBLIC-IP>`

---

### 3. Deploy AWS Lambda Function

1. Create a Lambda function with Python 3.8+.
2. Zip `lambda_function.py` with dependencies (`pandas`, `boto3`).
3. Upload zip to Lambda.
4. Set environment variables:

```
S3_BUCKET = synth-data-bucket
BEDROCK_REGION = us-east-1
```

5. Assign IAM Role with access to Bedrock, S3, API Gateway.

---

### 4. Configure API Gateway

1. Create REST API.
2. Add `POST` method connected to Lambda.
3. Deploy to `prod` stage.
4. Note the API URL and update it in `app.py`:

```python
API_URL = "https://<api-id>.execute-api.us-east-1.amazonaws.com/prod"
```

---

### 5. Create S3 Bucket

* Name: `synth-data-bucket`
* Region: `us-east-1`
* Keep bucket **private**
* Lambda handles access via **pre-signed URLs**

---

### 6. Enable Amazon Bedrock

* Make sure Bedrock is **enabled** in `us-east-1`
* Confirm access to relevant foundational models

---

## 🧪 Testing Instructions

1. Go to: `http://<EC2-PUBLIC-IP>`
2. Input parameters:

   * Rows: `100`
   * Columns: `age: integer (18–90), diagnosis: string (Healthy, Sick)`
3. Click **Generate**
4. Wait \~5–10 seconds for a **Download CSV** button
5. Click and download generated CSV
6. Open the file and verify values

---

## 📦 Expected Output

* A file named like: `synthetic_data_20250617.csv`
* Contains structured synthetic data reflecting provided schema
* Examples:

  ```
  age,diagnosis
  25,Healthy
  72,Sick
  ```

---

## ❗ Troubleshooting

| Issue               | Solution                                                           |
| ------------------- | ------------------------------------------------------------------ |
| App not loading     | Check EC2 port `80`, ensure Streamlit is running                   |
| API returns error   | Verify Lambda permissions and logs                                 |
| No Bedrock response | Ensure Bedrock is available and properly configured in `us-east-1` |
| CSV not downloading | Check if pre-signed URL is expired (valid for 1 hour)              |




> © 2025 Smart Synth Team. All rights reserved.

