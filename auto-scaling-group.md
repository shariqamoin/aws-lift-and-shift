# Auto Scaling Group Setup (Lift-and-Shift AWS Project)

## Overview

This module configures an AWS Auto Scaling Group (ASG) to automatically adjust the number of EC2 instances based on workload demand.

Auto Scaling improves application availability, fault tolerance, and scalability by dynamically launching and terminating instances based on CPU utilization metrics.

---

## Architecture Flow

Traffic increases
↓
CPU utilization rises
↓
Auto Scaling launches new EC2 instance
↓
Instance joins Load Balancer automatically

---

## Step 1 — Create AMI from Application Instance

Source instance:

app01

Created AMI:

vprofile-las-app-ami

Purpose:

Reusable deployment template for scaling operations.

---

## Step 2 — Create Launch Template

Launch template configuration includes:

AMI
Instance type
Security group
Key pair
Tags

Launch template name:

vprofile-las-app-LT

Purpose:

Ensures consistent configuration for new instances launched during scaling events.

---

## Step 3 — Create Auto Scaling Group

Auto Scaling group name:

vprofile-las-app-asg

Selected:

Launch Template
Existing Target Group

Availability zones:

Multiple AZs selected

Purpose:

Automatically manages EC2 instance lifecycle.

---

## Step 4 — Configure Load Balancer Health Checks

Enabled:

Elastic Load Balancing health checks

Purpose:

Detects application-level failures instead of instance-level failures.

Automatically replaces unhealthy instances.

---

## Step 5 — Configure Scaling Policy

Scaling metric used:

CPU Utilization

Threshold:

50%

Scaling behavior:

Scale out if CPU > 50%
Scale in if CPU < 50%

Capacity configuration:

Minimum instances: 1
Desired instances: 1
Maximum instances: 4

---

## Step 6 — Enable Session Stickiness

Enabled stickiness in target group settings.

Purpose:

Ensures user sessions remain connected to the same instance during authentication.

Required because application stores session locally.

---

## Step 7 — Transition to Auto Scaling Managed Instances

Removed manually created instance:

app01

Auto Scaling Group now manages instance lifecycle automatically.

---

## Key Learning Outcomes

- Created reusable AMI for scaling
- Configured Launch Template
- Implemented Auto Scaling Group
- Enabled load balancer health checks
- Configured CPU-based scaling policy
- Enabled session stickiness
- Automated EC2 lifecycle management