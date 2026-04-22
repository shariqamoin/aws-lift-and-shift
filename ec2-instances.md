# EC2 Instance Provisioning – vProfile Lift-and-Shift Deployment

This document explains the EC2 infrastructure provisioning strategy used to deploy the vProfile multi-tier application on AWS using a lift-and-shift approach.

Four EC2 instances were created to host application and backend services.

All services were provisioned automatically using user-data bootstrap scripts.

---

## Instance Architecture Overview

The following EC2 instances were deployed:

1. MySQL Database Server
2. Memcached Server
3. RabbitMQ Server
4. Tomcat Application Server

Each instance belongs to a specific security tier and uses automated configuration during launch.

---

## Security Group Mapping

Backend Instances

Assigned Security Group:

vprofile-backend-sg

Includes:

MySQL

Memcached

RabbitMQ

Application Instance

Assigned Security Group:

vprofile-app-sg

Includes:

Tomcat server

This ensures controlled communication between tiers.

---

## Source Code Repository

Application provisioning scripts were retrieved from:

https://github.com/hkhcoder/vprofile-project

Branch used:

awsliftandshift

Directory:

user-data/

Contains bootstrap scripts for:

mysql.sh

memcache.sh

rabbitmq.sh

tomcat_ubuntu.sh

These scripts automate service installation and configuration.

---

## Instance 1 – MySQL Database Server

Instance Name:

vprofile-db01

Operating System:

Amazon Linux 2023

Instance Type:

t2.micro (Free Tier Eligible)

Security Group:

vprofile-backend-sg

Provisioning Method:

User Data Script

Script:

mysql.sh

---

### MySQL Setup Tasks Performed Automatically

Install MariaDB server package

Start and enable database service

Secure database installation

Remove anonymous users

Remove test database

Create application database

Create admin user

Grant remote privileges

Deploy database schema

Database Name:

accounts

Admin User:

admin

---

## Instance 2 – Memcached Server

Instance Name:

vprofile-mc01

Operating System:

Amazon Linux 2023

Instance Type:

t2.micro

Security Group:

vprofile-backend-sg

Provisioning Method:

User Data Script

Script:

memcache.sh

---

### Memcached Setup Tasks Performed Automatically

Install memcached package

Start memcached service

Enable memcached service at boot

Update configuration to allow remote connections

Expose service on port:

11211

---

## Instance 3 – RabbitMQ Server

Instance Name:

vprofile-rmq01

Operating System:

Amazon Linux 2023

Instance Type:

t2.micro

Security Group:

vprofile-backend-sg

Provisioning Method:

User Data Script

Script:

rabbitmq.sh

---

### RabbitMQ Setup Tasks Performed Automatically

Import repository signing keys

Install Erlang dependency

Install RabbitMQ server

Start and enable service

Create application user

Assign administrator role

Grant permissions

Restart service

---

## Instance 4 – Tomcat Application Server

Instance Name:

vprofile-app01

Operating System:

Ubuntu 24.04 LTS

Instance Type:

t2.micro

Security Group:

vprofile-app-sg

Provisioning Method:

User Data Script

Script:

tomcat_ubuntu.sh

---

### Why Ubuntu Was Used for Application Tier

Ubuntu repository includes Tomcat package directly.

Installation command:

apt install tomcat10

This simplifies provisioning compared to CentOS manual installation.

Benefits:

Faster deployment

Simpler configuration

Reduced setup complexity

---

## Instance Tagging Strategy

Each instance included project-level tagging:

Tag Key:

project

Tag Value:

vprofile

Tags were also applied to attached EBS volumes.

Benefits:

Improved resource tracking

Cost allocation visibility

Safe infrastructure management

---

## Automated Provisioning Using User Data Scripts

User Data scripts executed during instance initialization performed:

Software installation

Configuration setup

Service startup

Dependency management

Database schema deployment

This eliminated the need for manual provisioning.

---

## Service Verification Steps

After provisioning, services were validated using SSH access.

Database Server Verification

Check service status:

systemctl status mariadb

Validate database:

mysql -u admin -p accounts

Memcached Verification

systemctl status memcached

RabbitMQ Verification

systemctl status rabbitmq-server

All services confirmed active and running successfully.

---

## SSH Access Configuration

Access method:

EC2 key pair authentication

Command example:

ssh -i vprofile-prod-key.pem ec2-user@instance-public-ip

Ubuntu instance login user:

ubuntu

Amazon Linux instance login user:

ec2-user

---

## Troubleshooting Strategy

If provisioning errors occur:

Terminate instance

Launch replacement instance

Verify correct:

AMI selection

Security group selection

User-data script usage

Outbound security group rules unchanged

Since provisioning is automated, recreating instances is faster than manual debugging.

---

## Architecture Outcome

Application services deployed successfully across:

Database tier

Cache tier

Message queue tier

Application tier

Provisioning completed using automated infrastructure setup aligned with cloud deployment best practices.
