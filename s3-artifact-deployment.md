# S3 Artifact Deployment (Lift-and-Shift AWS Project)

## Overview

This module implements artifact deployment using Amazon S3 as an intermediate storage layer between the local development environment and EC2 application servers.

The Java web application artifact (.war file) is built locally using Maven and uploaded to an S3 bucket. The EC2 instance retrieves the artifact securely using an IAM role and deploys it to Apache Tomcat.

This simulates a real-world deployment pipeline used in lift-and-shift migration architectures.

---

## Architecture Flow

Local Machine
↓
Build Artifact (Maven)
↓
Upload Artifact to S3
↓
EC2 retrieves Artifact using IAM Role
↓
Deploy to Apache Tomcat

---

## Prerequisites

Installed locally:

- Java JDK 17
- Maven 3.9+
- AWS CLI

Verify installations:

java -version
mvn -version
aws --version


---

## Step 1 — Create S3 Bucket

Created S3 bucket for storing application artifacts:

Example:

vprofile-las-artifacts-<unique-id>


Purpose:

Stores deployable WAR files accessible by EC2 instances.

---

## Step 2 — Create IAM User for Local Machine Access

Created IAM user:

vprofile-s3-admin

Attached policy:

AmazonS3FullAccess


Generated credentials:

Access Key
Secret Key

Configured locally:

aws configure


Purpose:

Allows secure upload of artifacts from local machine to S3.

---

## Step 3 — Create IAM Role for EC2 Access

Created role:

S3-admin-role


Attached policy:

AmazonS3FullAccess

Assigned role to EC2 instance:

app01


Purpose:

Allows EC2 instance to retrieve artifacts securely without storing credentials.

---

## Step 4 — Build Application Artifact

Navigate to project directory:

mvn install

Generated artifact:

target/vprofile-v2.war

---

## Step 5 — Upload Artifact to S3

Upload artifact:

aws s3 cp target/vprofile-v2.war s3://<bucket-name>/

Verify upload:

aws s3 ls s3://<bucket-name>/

---

## Step 6 — Download Artifact on EC2 Instance

Connect to EC2:

ssh -i key.pem ubuntu@<public-ip>

Install AWS CLI:

snap install aws-cli --classic

Download artifact:

aws s3 cp s3://<bucket-name>/vprofile-v2.war /tmp/

---

## Step 7 — Deploy Artifact to Apache Tomcat

Stop Tomcat:

sudo systemctl stop tomcat10

Remove default ROOT application:

sudo rm -rf /var/lib/tomcat10/webapps/ROOT

Deploy new artifact:

sudo cp /tmp/vprofile-v2.war /var/lib/tomcat10/webapps/ROOT.war

Start Tomcat:

sudo systemctl start tomcat10


Application deployed successfully.

---

## Key Learning Outcomes

- Built Java artifact using Maven
- Uploaded artifact to Amazon S3
- Configured IAM user authentication
- Configured IAM role-based EC2 access
- Deployed WAR file to Apache Tomcat
- Implemented cloud-native artifact delivery workflow