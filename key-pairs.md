# EC2 Key Pair Configuration – vProfile Deployment

This document explains the key pair setup used for secure access to EC2 instances during the lift-and-shift deployment of the vProfile application.

Key pairs provide secure SSH authentication without using passwords.

---

## Key Pair Name

vprofile-prod-key

Format:

.pem

Used with:

Linux / macOS Terminal

Git Bash (Windows)

Alternative format:

.ppk

Used with:

PuTTY (Windows)

---

## Purpose of Key Pair

The key pair is required to:

Securely connect to EC2 instances

Deploy application artifacts

Configure backend services

Perform administrative operations

Monitor instance setup

---

## Why Key Pairs Are Important

Key pairs improve infrastructure security by:

Eliminating password-based login

Reducing brute-force attack risk

Providing encrypted authentication

Supporting automated deployments

---

## Usage Workflow

Step 1:

Create key pair in AWS EC2 console

Step 2:

Download key file locally

Step 3:

Secure file permissions

Example:

chmod 400 vprofile-prod-key.pem

Step 4:

Connect to EC2 instance using SSH

Example:

ssh -i vprofile-prod-key.pem ec2-user@instance-public-ip

---

## Security Best Practices

Never upload private key to GitHub

Store key securely

Restrict file permissions

Rotate keys periodically in production environments

Use IAM roles for automation instead of SSH where possible
