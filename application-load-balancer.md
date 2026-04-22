# Application Load Balancer Setup (Lift-and-Shift AWS Project)

## Overview

This module configures an AWS Application Load Balancer (ALB) to distribute incoming HTTP traffic across EC2 instances running Apache Tomcat.

The load balancer improves application availability, scalability, and reliability by routing requests through a target group and performing health checks on backend instances.

---

## Architecture Flow

User
↓
Application Load Balancer (Port 80 / 443)
↓
Target Group
↓
EC2 Instance (Tomcat running on Port 8080)

---

## Step 1 — Verify Application Availability

Before configuring the load balancer, confirm the application is accessible directly from EC2.

Allow inbound rule:

Port 8080 from My IP

Test access:

http://<EC2-public-ip>:8080


Application should load successfully.

---

## Step 2 — Create Target Group

Configuration:

Target Type: Instances
Protocol: HTTP
Port: 8080

Health check configuration:

Protocol: HTTP
Port: 8080


Register instance:

app01

Purpose:

Routes traffic from load balancer to EC2 instance.

---

## Step 3 — Create Application Load Balancer

Load balancer type:

Application Load Balancer

Listeners configured:

HTTP : 80
HTTPS : 443 (optional)

Network configuration:

Default VPC
All availability zones selected

Security group attached:

load-balancer-security-group

Target group attached:

vprofile-las-tg

---

## Step 4 — Configure HTTPS (Optional)

Configured SSL certificate using AWS Certificate Manager (ACM).

Attached certificate to:

HTTPS Listener (Port 443)

Enables secure encrypted connection to application.

---

## Step 5 — Configure DNS Mapping (Optional)

Created CNAME record:

vprofileapp.example.com

Mapped to:

load-balancer-dns-name

Alternative access method without domain:

http://<load-balancer-dns>

---

## Step 6 — Verify Target Health Status

Navigate to:

Target Groups → Targets

Expected status:

Healthy


If unhealthy:

Check:

- Tomcat service running
- Security group allows port 8080
- Health check configuration correct

---

## Key Learning Outcomes

- Created Application Load Balancer
- Configured listeners and routing rules
- Implemented health checks
- Integrated EC2 instances via target groups
- Enabled HTTPS using ACM certificate
- Configured DNS routing using CNAME record