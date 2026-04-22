# AWS Lift-and-Shift Application Deployment (vProfile)

This project demonstrates a Lift-and-Shift migration strategy by deploying the multi-tier vProfile application from a traditional virtualized environment to AWS cloud infrastructure.

The goal of this project is to simulate a production-style AWS deployment using Infrastructure-as-a-Service principles and core AWS services such as EC2, S3, Route 53, Load Balancer, and Auto Scaling.

---

## Project Objective

The objective of this project is to:

• Migrate an existing application stack from on-premise environment to AWS  
• Implement scalable infrastructure using AWS services  
• Improve flexibility using elastic cloud architecture  
• Reduce infrastructure management complexity  
• Enable high availability using Load Balancer and Auto Scaling  
• Implement private DNS architecture using Route 53  
• Store application artifacts using S3  
• Prepare infrastructure for automation and Infrastructure-as-Code

---

## Architecture Diagram

![Architecture Diagram](screenshots/architecture.png)

## Application Architecture

The application follows a multi-tier architecture:

Frontend Layer:
Application Load Balancer (HTTPS)

Application Layer:
Apache Tomcat running vProfile application

Backend Layer:
MySQL
Memcached
RabbitMQ

Supporting Services:
Route 53 Private DNS
S3 Artifact Storage
IAM
Security Groups

---

## AWS Services Used

Compute:
Amazon EC2

Storage:
Amazon S3
Amazon EBS

Networking:
Security Groups
Application Load Balancer

DNS:
Amazon Route 53 (Private Hosted Zone)

Security:
IAM

Scaling:
Auto Scaling Group

---

## Architecture Workflow

User → Domain Endpoint → Application Load Balancer (HTTPS)

Load Balancer → Routes traffic to Tomcat EC2 instances

Tomcat Instances → Access backend services via Route 53 Private DNS

Backend Services:
MySQL
Memcached
RabbitMQ

Artifacts:
Stored in S3 and deployed to Tomcat instances

Auto Scaling Group:
Automatically scales application tier based on demand

---

## Lift-and-Shift Strategy Explained

Lift-and-Shift means migrating an application from an on-premise environment to cloud infrastructure without modifying application architecture.

Original Deployment:

Local VMs using:
Nginx
Apache Tomcat
MySQL
Memcached
RabbitMQ

New Deployment:

AWS Cloud using:
EC2 Instances
Application Load Balancer
Route 53
S3 Storage
Auto Scaling

This approach minimizes migration complexity and speeds up cloud adoption.

---

## Execution Flow

1. Create Key Pair for EC2 access
2. Configure Security Groups
3. Launch Backend EC2 Instances
4. Configure Route 53 Private Hosted Zone
5. Build Application Artifact Locally
6. Upload Artifact to S3 Bucket
7. Deploy Artifact to Tomcat Instances
8. Configure Application Load Balancer with HTTPS
9. Map Domain Endpoint to Load Balancer
10. Configure Auto Scaling Group

---

## Security Design

Three Security Groups were created:

Load Balancer Security Group
Allows HTTPS traffic from internet

Tomcat Security Group
Allows port 8080 access from Load Balancer only

Backend Services Security Group
Allows database/cache/message broker access from Tomcat instances only

This ensures secure tier-based communication architecture.

---

## Artifact Deployment Strategy

Application artifacts are:

Built locally
Uploaded to S3 bucket
Downloaded into Tomcat EC2 instances
Deployed inside application container

This simulates a production deployment workflow.

---

## Project Benefits

Flexible infrastructure

Pay-as-you-go pricing model

Improved scalability

Reduced operational complexity

Cloud-ready architecture

Foundation for Infrastructure-as-Code automation
