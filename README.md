# AWS Application Load Balancer (ALB) - Zero Downtime Deployment

![AWS](https://img.shields.io/badge/AWS-EC2-orange)
![Load Balancer](https://img.shields.io/badge/Application-Load%20Balancer-blue)
![Deployment](https://img.shields.io/badge/Deployment-Zero%20Downtime-success)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

---

## 📌 Project Overview

This project demonstrates how to perform a **Zero Downtime Deployment** using **AWS Application Load Balancer (ALB)** and **Weighted Target Groups**.

Initially, the first version of the website is deployed on two EC2 instances. A second version of the website is deployed on two new EC2 instances. Instead of changing the website URL, traffic is shifted from the old servers to the new servers by updating the ALB listener rule and changing target group weights.

Users continue using the **same ALB DNS URL**, resulting in a seamless deployment without any downtime.

---

# 🎯 Objective

- Deploy a website on multiple EC2 instances.
- Configure Security Groups.
- Create Target Groups.
- Configure an Application Load Balancer.
- Deploy an updated website on new EC2 instances.
- Shift traffic using Weighted Target Groups.
- Perform Zero Downtime Deployment.

---

# 🏗️ AWS Services Used

- Amazon EC2
- Application Load Balancer (ALB)
- Target Groups
- Security Groups
- VPC
- HTTP Listener

---

# 📁 Project Structure

```text
AWS-ALB-Zero-Downtime-Deployment/
│
├── README.md
├── CHANGELOG.md
│
├── docs/
│   ├── Project-Documentation.md
│   └── Architecture.md
│
├── screenshots/
│
├── website-v1/
│
└── website-v2/
```

---

# ⚙️ Project Workflow

### Step 1

Launch two EC2 instances.

- Server 1
- Server 2

---

### Step 2

Deploy Website Version 1.

---

### Step 3

Configure Security Group.

Allow

- HTTP (80)
- HTTPS (443)

---

### Step 4

Create Target Group

```
TG-Web-V1
```

Register

- Server 1
- Server 2

---

### Step 5

Create Application Load Balancer

```
MyALB
```

Attach

```
TG-Web-V1
```

---

### Step 6

Verify Website Version 1.

Open

```
http://<ALB-DNS-Name>
```

---

### Step 7

Launch

- Server 3
- Server 4

---

### Step 8

Deploy Website Version 2.

---

### Step 9

Create Target Group

```
TG-Web-V2
```

Register

- Server 3
- Server 4

---

### Step 10

Edit Listener Rule.

Add

```
TG-Web-V2
```

---

### Step 11

Configure Weighted Routing

| Target Group | Weight |
|--------------|-------:|
| TG-Web-V1 | 0 |
| TG-Web-V2 | 100 |

---

### Step 12

Refresh the ALB URL.

The website changes to Version 2 without changing the URL.

---

# 📸 Screenshots

The complete implementation screenshots are available in the **screenshots/** directory.

- EC2 Instances
- Security Groups
- Target Groups
- Healthy Targets
- Application Load Balancer
- Listener Rules
- Website Version 1
- Website Version 2
- Weighted Routing

---

# 📚 Key Learnings

- Launching Amazon EC2 instances
- Hosting a website on EC2
- Configuring Security Groups
- Creating Target Groups
- Registering EC2 instances
- Creating an Application Load Balancer
- Configuring Listener Rules
- Weighted Target Group Routing
- Zero Downtime Deployment
- Blue-Green Deployment Concept

---

# ✅ Result

Successfully deployed an updated version of the website without changing the URL and without any downtime by using an AWS Application Load Balancer and Weighted Target Groups.

---

# 👨‍💻 Author

**Samir Shaikh**

