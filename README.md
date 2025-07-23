
# 📁 Serverless File Sharing Platform

This project is a step-by-step guide to building a **Serverless File Sharing Platform** using **AWS Lambda**, **Amazon S3**, and **API Gateway**. The platform enables users to securely upload and download files via a RESTful HTTP API using clients like Postman or curl.

---

## 📝 Project Description

The Serverless File Sharing Platform allows users to securely upload and download files via a simple HTTP API. It leverages:
- **AWS Lambda** for serverless compute
- **API Gateway** for RESTful API management
- **Amazon S3** for durable and scalable object storage

Users can interact with the platform using any HTTP client, making it versatile for various use cases involving file sharing and storage.

---

## 📌 Use Cases

- **File Sharing**: Securely share documents, images, or any digital assets.
- **File Distribution**: Generate download links for distributing media or software.
- **Collaborative Work**: Teams can share documents and resources securely across locations.

---

## 🏗️ Project Architecture

```
Client (Postman/curl) --> API Gateway --> Lambda --> Amazon S3
```

---

## ✅ Prerequisites

- AWS account with permission to create:
  - Lambda functions
  - API Gateway endpoints
  - S3 buckets

---

## 🚀 Steps to Deploy

### Step 1: Create an S3 bucket
```bash
aws s3 mb s3://myfilesharingbucket
```

### Step 2: Create Lambda Function for Uploads
- **Name**: `UploadFunction`
- **Runtime**: Python 3.x
- **Execution Role**: IAM role with S3 write permissions
- **Code**: Your Python code for handling uploads

### Step 3: Create Lambda Function for Downloads
- **Name**: `DownloadFunction`
- **Runtime**: Python 3.x
- **Execution Role**: IAM role with S3 read permissions
- **Code**: Your Python code for handling downloads

### Step 4: Create an API Gateway
- **Name**: `my-file-sharing-api-amc`
- Create resource: `/files`
  - Method: `POST` → integrates with `UploadFunction`
  - Method: `GET` → integrates with `DownloadFunction`

---

## ⚙️ Configure API Gateway

### Step 5: Configure GET Method
- Method Request:
  - Request Validator → Validate Query String Parameters and Headers
  - Request Body: `text/plain`
- Integration Request:
  - Mapping Templates → Content-Type: `application/json`
```json
{
  "queryStringParameters": {
    "fileName": "$input.params('fileName')"
  }
}
```

### Step 6: Configure POST Method
- Integration Request:
  - Mapping Templates → Content-Type: `text/plain`
```json
{
  "body" : "$input.body",
  "queryStringParameters" : {
    "fileName" : "$input.params('fileName')"
  }
}
```

---

### Step 7: Deploy the API
- In API Gateway console:
  - Click on `Actions` → `Deploy API`
  - Choose a stage name (e.g., `dev`) and deploy

---

## 🧪 Testing the Platform

### 1. Upload a File

**Using Postman**:
- Method: `POST`
- URL: `https://<api-id>.execute-api.<region>.amazonaws.com/dev/files?fileName=test.txt`
- Body: Raw, Text → `Hello World!`

**Using curl**:
```bash
curl --location 'https://<api-id>.execute-api.<region>.amazonaws.com/dev/files?fileName=test.txt' \
--header 'Content-Type: text/plain' \
--data 'Hello World from A Monk in Cloud!'
```

---

### 2. Download a File

**Using Postman or curl**:
```bash
curl --location 'https://<api-id>.execute-api.<region>.amazonaws.com/dev/files?fileName=test.txt'
```

---
