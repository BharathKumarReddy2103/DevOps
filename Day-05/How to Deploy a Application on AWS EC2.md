# Deploying a Node.js Application on AWS EC2: A Beginner-Friendly Guide

## Introduction

In the modern DevOps and Cloud ecosystem, understanding how to deploy applications on cloud platforms like AWS is a must-have skill. This guide walks you through the complete lifecycle of deploying a Node.js application on an AWS EC2 instance, from cloning the source code to making the app accessible via the internet.

Whether you're a student, aspiring DevOps Engineer, or a beginner in cloud computing, this guide will provide the practical steps and real-world concepts needed to launch your first application on AWS.

---

## Table of Contents

1. [Prerequisites](#prerequisites)  
2. [Introduction to IAM and EC2](#introduction-to-iam-and-ec2)  
3. [Cloning and Running the App Locally](#cloning-and-running-the-app-locally)  
4. [Setting Up AWS IAM and EC2](#setting-up-aws-iam-and-ec2)  
5. [Connecting to EC2 via SSH](#connecting-to-ec2-via-ssh)  
6. [Installing Required Dependencies](#installing-required-dependencies)  
7. [Deploying the Node.js App on EC2](#deploying-the-nodejs-app-on-ec2)  
8. [Exposing the App to the Internet](#exposing-the-app-to-the-internet)  
9. [Best Practices](#best-practices)  
10. [Conclusion](#conclusion)

---

## Prerequisites

Before getting started, ensure you have:

- A GitHub account  
- AWS Free Tier account  
- Basic knowledge of Git and Linux commands  
- Visual Studio Code or any code editor  
- Familiarity with terminal usage

---

## Introduction to IAM and EC2

**IAM (Identity and Access Management)** helps manage users and permissions within your AWS account. It's a security best practice to avoid using the root user and instead create IAM users with specific roles.

**EC2 (Elastic Compute Cloud)** is AWS's virtual server offering that allows you to host applications on cloud infrastructure.

---

## Cloning and Running the App Locally

### Step 1: Clone the GitHub Repository

```bash
git clone https://github.com/<your-repo>/aws-nodejs-demo.git
cd aws-nodejs-demo
```

### Step 2: Set Up Environment Variables

Create a `.env` file and add necessary environment variables (used for Stripe payment integration in this case).

```env
DOMAIN=http://localhost
PORT=3000
STRIPE_PUBLISHABLE_KEY=your_publishable_key
STRIPE_SECRET_KEY=your_secret_key
```

### Step 3: Install Dependencies and Start the App

```bash
npm install
npm run start
```

Visit `http://localhost:3000` to view the application running locally.

---

## Setting Up AWS IAM and EC2

### Step 1: Create an IAM User

- Go to IAM → Add User  
- Enable console access  
- Assign **AdministratorAccess** (for learning only)  
- Note the login URL and credentials

### Step 2: Launch an EC2 Instance

- Search for EC2 → Launch Instance  
- Select Ubuntu 22.04 LTS  
- Choose **t2.micro** (Free Tier eligible)  
- Create a new key pair (`demo.pem`)  
- Allow SSH traffic (port 22)  
- Launch the instance

---

## Connecting to EC2 via SSH

Download the `.pem` file and use the following command:

```bash
chmod 400 demo.pem
ssh -i "demo.pem" ubuntu@<ec2-public-ip>
```

Once connected, you're inside the remote server.

---

## Installing Required Dependencies

### Update System Packages

```bash
sudo apt update
```

### Install Node.js and npm

```bash
sudo apt install nodejs
sudo apt install npm
```

### Verify Installation

```bash
node -v
npm -v
```

---

## Deploying the Node.js App on EC2

### Step 1: Clone the Repository

```bash
git clone https://github.com/<your-repo>/aws-nodejs-demo.git
cd aws-nodejs-demo
```

### Step 2: Add Environment Variables

```bash
touch .env
vim .env
```

Paste your variables and save (`:wq` to exit Vim):

```env
DOMAIN=http://<ec2-public-ip>
PORT=3000
STRIPE_PUBLISHABLE_KEY=your_publishable_key
STRIPE_SECRET_KEY=your_secret_key
```

### Step 3: Install Dependencies and Start the App

```bash
npm install
npm run start
```

---

## Exposing the App to the Internet

### Configure Security Groups

1. Go to EC2 → Security Groups  
2. Edit **Inbound Rules**  
3. Add a new rule:
   - Type: **Custom TCP**
   - Port: `3000`
   - Source: `0.0.0.0/0` (Public internet)

### Access the App

Visit:

```
http://<ec2-public-ip>:3000
```

You should see your live Node.js application running from AWS

---

## Best Practices

- Never expose sensitive `.env` files in public repositories  
- Use IAM roles and least privilege access in production  
- Use HTTPS and domain names (via Route 53 + SSL) for real-world deployments  
- Monitor EC2 costs with AWS Budgets  
- Use Docker and CI/CD pipelines for scalable deployments

---

## Conclusion

Deploying a Node.js application on AWS EC2 is a foundational skill in modern DevOps. This guide demonstrated how to:

- Set up IAM roles  
- Launch and connect to EC2  
- Configure your app and environment  
- Securely expose services to the internet

With these basics, you’re now empowered to host your own applications, practice DevOps workflows, and build impressive projects for your GitHub profile and resume.
