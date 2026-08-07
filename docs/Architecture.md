# AWS Architecture

## Project Title

**AWS Application Load Balancer (ALB) - Zero Downtime Deployment Using Weighted Target Groups**

---

# Architecture Overview

This project demonstrates a **Zero Downtime Deployment** using an **AWS Application Load Balancer (ALB)** and **Weighted Target Groups**.

Initially, users access **Website Version 1** hosted on two EC2 instances. A new version of the website is deployed on two additional EC2 instances. Instead of changing the application's URL or stopping the existing servers, the Application Load Balancer shifts traffic from the old Target Group to the new Target Group by adjusting the target group weights.

As a result, users continue accessing the application through the **same ALB DNS URL**, while the updated version is served without any service interruption.

---

# Architecture Diagram

```
                        Internet Users
                               │
                               ▼
               +-------------------------------+
               | Application Load Balancer     |
               |           (MyALB)             |
               +-------------------------------+
                               │
                ┌──────────────┴──────────────┐
                │                             │
                ▼                             ▼
      +------------------+         +------------------+
      |    TG-Web-V1     |         |    TG-Web-V2     |
      |    Weight = 0    |         |   Weight = 100   |
      +------------------+         +------------------+
                │                             │
        ┌───────┴────────┐           ┌────────┴────────┐
        ▼                ▼           ▼                 ▼
 +---------------+ +---------------+ +---------------+ +---------------+
 |   Server 1    | |   Server 2    | |   Server 3    | |   Server 4    |
 | Website V1    | | Website V1    | | Website V2    | | Website V2    |
 +---------------+ +---------------+ +---------------+ +---------------+
```

---

# Components Used

## 1. Amazon EC2

Four EC2 instances are used in this project.

| Instance | Purpose |
|----------|---------|
| Server 1 | Website Version 1 |
| Server 2 | Website Version 1 |
| Server 3 | Website Version 2 |
| Server 4 | Website Version 2 |

---

## 2. Security Groups

The EC2 instances allow the following inbound traffic:

| Protocol | Port | Source |
|----------|------|--------|
| HTTP | 80 | Anywhere |
| HTTPS | 443 | Anywhere |

This allows users to access the hosted website.

---

## 3. Target Groups

### TG-Web-V1

Contains:

- Server 1
- Server 2

Initially receives application traffic.

---

### TG-Web-V2

Contains:

- Server 3
- Server 4

Receives all traffic after deployment.

---

## 4. Application Load Balancer (ALB)

The Application Load Balancer performs the following tasks:

- Receives incoming HTTP requests.
- Distributes traffic across registered EC2 instances.
- Routes requests to the appropriate Target Group.
- Enables Zero Downtime Deployment using Weighted Routing.
- Maintains a single public DNS name for users.

---

# Traffic Flow

## Initial Deployment

```
User
   │
   ▼
Application Load Balancer
   │
   ▼
TG-Web-V1
   │
 ┌─┴────────┐
 ▼          ▼
Server1   Server2
```

Website Version 1 is served to all users.

---

## After Deployment

```
User
   │
   ▼
Application Load Balancer
   │
   ▼
TG-Web-V2
   │
 ┌─┴────────┐
 ▼          ▼
Server3   Server4
```

Website Version 2 is served to all users.

---

# Weighted Routing

The ALB Listener Rule is configured with the following weights:

| Target Group | Weight |
|--------------|--------|
| TG-Web-V1 | 0 |
| TG-Web-V2 | 100 |

This means:

- 0% of incoming requests are sent to TG-Web-V1.
- 100% of incoming requests are sent to TG-Web-V2.

---

# Why Zero Downtime?

The website URL does not change because users access the **Application Load Balancer** instead of individual EC2 instances.

During deployment:

- New servers are prepared in advance.
- The updated website is deployed to the new servers.
- Target Group weights are modified.
- The ALB immediately starts forwarding requests to the new Target Group.

Since the ALB remains available throughout the deployment, users experience no downtime.

---

# Benefits

- High Availability
- Zero Downtime Deployment
- Easy Rollback
- Scalable Architecture
- Seamless User Experience
- Centralized Traffic Management

---

# Conclusion

This project demonstrates how an AWS Application Load Balancer can be used with multiple Target Groups to perform a Zero Downtime Deployment. By shifting traffic from the old Target Group to the new Target Group using weighted routing, the updated application becomes available instantly without changing the application URL or interrupting user access.