# Route 53 Private DNS Configuration – vProfile Lift-and-Shift Deployment

This document explains how AWS Route 53 Private Hosted Zone was configured to provide internal DNS resolution between application and backend services in the vProfile deployment architecture.

The purpose of this setup is to enable service discovery using hostnames instead of static IP addresses.

---

## Why Private DNS Was Required

The vProfile application running on Tomcat connects to backend services:

MySQL
Memcached
RabbitMQ

These services are referenced inside:

application.properties

Example:

db01.vprofile.in
mc01.vprofile.in
rmq01.vprofile.in

Using hostnames instead of IP addresses ensures infrastructure flexibility and supports instance replacement without modifying application configuration.

---

## Problem Without DNS Resolution

If backend services were configured using static IP addresses:

Any instance replacement would require updating configuration files

Manual maintenance would increase

Infrastructure scalability would be reduced

Automation would become difficult

Therefore hostname-based resolution was implemented using Route 53 Private Hosted Zone.

---

## Service Used

Amazon Route 53 Private Hosted Zone

Purpose:

Internal DNS resolution inside AWS VPC

Scope:

Accessible only by EC2 instances inside the selected VPC

Not publicly accessible from internet

---

## Hosted Zone Configuration

Hosted Zone Name:

vprofile.in

Type:

Private Hosted Zone

Region:

Same region as EC2 instances

Associated VPC:

Default VPC used for deployment

---

## DNS Records Created

The following A-record mappings were configured.

A-record means:

Hostname → Private IP Address mapping

---

### Database Server Mapping

Hostname:

db01.vprofile.in

Mapped To:

Private IP address of

vprofile-db01 instance

Used By:

Tomcat application server

Purpose:

Resolve database service location

---

### Memcached Server Mapping

Hostname:

mc01.vprofile.in

Mapped To:

Private IP address of

vprofile-mc01 instance

Used By:

Tomcat application server

Purpose:

Resolve caching service location

---

### RabbitMQ Server Mapping

Hostname:

rmq01.vprofile.in

Mapped To:

Private IP address of

vprofile-rmq01 instance

Used By:

Tomcat application server

Purpose:

Resolve messaging service location

---

### Application Server Mapping (Optional)

Hostname:

app01.vprofile.in

Mapped To:

Private IP address of

vprofile-app01 instance

Purpose:

Optional internal reference

Not required for load balancer routing

Included for completeness of internal DNS architecture

---

## Why A-Records Were Used

Record Type:

A Record

Purpose:

Maps hostname directly to IP address

Example:

db01.vprofile.in → 172.x.x.x

Alternative:

CNAME records map hostname to hostname instead of IP

Example:

app.example.com → alb.example.com

Since backend services use instance-level IP mapping, A-records were required.

---

## DNS Resolution Validation

DNS resolution was verified from the Tomcat instance using SSH.

Example command:

ping -c 4 db01.vprofile.in

Validation confirmed:

Hostname resolves successfully

Correct private IP address returned

Network connectivity established

Same validation steps were performed for:

mc01.vprofile.in

rmq01.vprofile.in

---

## Architecture Benefit

Using Route 53 Private Hosted Zone provides:

Service discovery support

Improved infrastructure flexibility

Simplified instance replacement

Clean configuration management

Production-style internal networking architecture

Support for scalable cloud deployment design

---

## Best Practices Followed

Used hostname-based service references instead of static IPs

Restricted DNS scope to private VPC only

Maintained environment consistency

Verified DNS resolution from application tier

Ensured correct mapping with private IP addresses only
