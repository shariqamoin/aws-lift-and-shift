# Security Groups Configuration – vProfile Lift-and-Shift Deployment

This document explains the security group architecture used in the AWS lift-and-shift deployment of the vProfile application.

Security Groups act as virtual firewalls controlling inbound and outbound traffic to EC2 instances.

The architecture follows a multi-tier security model to ensure controlled communication between infrastructure layers.

---

## Security Group Architecture Overview

Three security groups were created:

1. Load Balancer Security Group
2. Application (Tomcat) Security Group
3. Backend Services Security Group

Each tier communicates only with required services using restricted ports.

This approach follows the Principle of Least Privilege.

---

## 1. Load Balancer Security Group

Name:

vprofile-ELB-SG

Purpose:

Allow secure internet traffic to the Application Load Balancer.

Inbound Rules:

HTTP (Port 80) → Allowed from Anywhere (IPv4 + IPv6)

HTTPS (Port 443) → Allowed from Anywhere (IPv4 + IPv6)

Outbound Rules:

Default (Allow all)

This ensures connectivity between Load Balancer and application servers.

---

## 2. Application Security Group (Tomcat Instances)

Name:

vprofile-app-sg

Purpose:

Allow traffic from Load Balancer to Tomcat instances hosting vProfile application.

Inbound Rules:

Port 8080 (Custom TCP)

Source:

Load Balancer Security Group

Purpose:

Allow application traffic from Load Balancer only.

Port 22 (SSH)

Source:

My IP

Purpose:

Secure administrative access to EC2 instances.

Outbound Rules:

Default (Allow all)

Required for backend communication.

---

## 3. Backend Services Security Group

Name:

vprofile-backend-sg

Purpose:

Allow communication between application servers and backend infrastructure services.

Backend services include:

MySQL
Memcached
RabbitMQ

Inbound Rules:

MySQL

Port 3306

Source:

Application Security Group

Memcached

Port 11211

Source:

Application Security Group

RabbitMQ

Port 5672

Source:

Application Security Group

SSH Access

Port 22

Source:

My IP

Used for administrative access.

---

## Backend Internal Communication Rule

Additional rule configured:

Allow All Traffic

Source:

vprofile-backend-sg (self-reference)

Purpose:

Allow backend services to communicate with each other internally.

Example:

Memcached → MySQL

RabbitMQ → MySQL

This is safe because access remains restricted inside the same security group.

---

## Why Tier-Based Security Groups Matter

This layered configuration ensures:

Restricted access between tiers

Improved infrastructure security

Controlled service-to-service communication

Protection against unauthorized external traffic

Compliance with cloud security best practices

---

## Security Best Practices Followed

Principle of Least Privilege enforced

Direct internet access blocked for backend services

Only Load Balancer exposed publicly

SSH restricted to administrator IP only

Inter-service communication limited to required ports
