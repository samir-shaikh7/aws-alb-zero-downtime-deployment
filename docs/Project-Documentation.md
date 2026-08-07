# AWS Practical: Zero Downtime Deployment Using Application Load Balancer (ALB)

## Project Objective

The objective of this project is to deploy a website on Amazon EC2 instances behind an Application Load Balancer (ALB) and perform a **Zero Downtime Deployment** using **Weighted Target Groups**. Instead of replacing the existing servers, a new version of the website is deployed on new EC2 instances, and traffic is shifted seamlessly through the ALB without changing the application URL.

---

# Prerequisites

Before starting this project, ensure you have:

- An AWS Account
- A configured VPC
- Two Public Subnets
- A Key Pair
- Basic knowledge of EC2 and Security Groups

---

# Step 1: Launch EC2 Instances (Version 1)

Navigate to:

AWS Console → EC2 → Instances → Launch Instance

Launch two EC2 instances.

| Instance Name | Purpose |
|---------------|---------|
| Server 1 | Website Version 1 |
| Server 2 | Website Version 1 |

Wait until both instances reach the **Running** state.

---

# Step 2: Deploy Website Version 1

Connect to both EC2 instances.

Upload the first version of the website to:

- Server 1
- Server 2

Verify that both servers display the same website.

---

# Step 3: Configure Security Group

Edit the Security Group attached to the EC2 instances.

Add the following inbound rules:

| Type | Port | Source |
|------|------|--------|
| HTTP | 80 | Anywhere (0.0.0.0/0) |
| HTTPS | 443 | Anywhere (0.0.0.0/0) |

Save the Security Group.

---

# Step 4: Create Target Group (TG-Web-V1)

Navigate to:

EC2 → Target Groups

Create a new Target Group.

**Target Group Name**

```
TG-Web-V1
```

Register the following instances:

- Server 1
- Server 2

Complete the Target Group creation.

Wait until both targets become **Healthy**.

---

# Step 5: Create Application Load Balancer

Navigate to:

EC2 → Load Balancers

Create an **Application Load Balancer**.

Configuration:

| Property | Value |
|----------|-------|
| Name | MyALB |
| Listener | HTTP (80) |
| Target Group | TG-Web-V1 |

Create the Load Balancer.

Wait until the ALB status becomes **Active**.

---

# Step 6: Verify Website Version 1

Copy the DNS Name of the Application Load Balancer.

Example:

```
http://myalb-xxxxxxxx.ap-south-1.elb.amazonaws.com
```

Open the DNS URL in a web browser.

Verify that Website Version 1 loads successfully.

---

# Step 7: Launch EC2 Instances (Version 2)

Launch two additional EC2 instances.

| Instance Name | Purpose |
|---------------|---------|
| Server 3 | Website Version 2 |
| Server 4 | Website Version 2 |

Wait until both instances are in the **Running** state.

---

# Step 8: Deploy Website Version 2

Upload the updated version of the website to:

- Server 3
- Server 4

Verify that both servers serve the updated website.

---

# Step 9: Create Target Group (TG-Web-V2)

Navigate to:

EC2 → Target Groups

Create another Target Group.

**Target Group Name**

```
TG-Web-V2
```

Register:

- Server 3
- Server 4

Wait until both targets become **Healthy**.

---

# Step 10: Configure ALB Listener Rules

Navigate to:

EC2 → Load Balancers

Select:

```
MyALB
```

Open:

Listeners and Rules

Select:

```
HTTP : 80
```

Click:

```
Edit Rules
```

Edit the default forwarding rule.

Add:

```
TG-Web-V2
```

Now both Target Groups appear in the forwarding rule.

---

# Step 11: Configure Weighted Routing

Assign the following weights:

| Target Group | Weight |
|--------------|--------|
| TG-Web-V1 | 0 |
| TG-Web-V2 | 100 |

Save the rule.

This configuration routes all incoming traffic to the new target group.

---

# Step 12: Verify Zero Downtime Deployment

Open the same Application Load Balancer DNS URL.

Refresh the browser.

The website now displays **Website Version 2**.

Notice that:

- The URL remains the same.
- No downtime occurs.
- Users are automatically redirected to the new servers.

---

# Project Workflow

```
Launch Server 1 & Server 2
        │
        ▼
Deploy Website Version 1
        │
        ▼
Create TG-Web-V1
        │
        ▼
Create MyALB
        │
        ▼
Verify Website V1
        │
        ▼
Launch Server 3 & Server 4
        │
        ▼
Deploy Website Version 2
        │
        ▼
Create TG-Web-V2
        │
        ▼
Edit Listener Rule
        │
        ▼
TG-Web-V1 → Weight 0
TG-Web-V2 → Weight 100
        │
        ▼
Refresh ALB URL
        │
        ▼
Website Version 2 Live
```

---

# Result

The updated website was deployed successfully without changing the application URL and without causing any downtime. The Application Load Balancer redirected all traffic from the old Target Group to the new Target Group using weighted routing.

---

# Key Learnings

- Launching EC2 instances
- Deploying static websites on EC2
- Configuring Security Groups
- Creating Target Groups
- Registering EC2 instances with Target Groups
- Creating an Application Load Balancer
- Configuring ALB Listener Rules
- Implementing Weighted Target Groups
- Performing Zero Downtime Deployment
- Understanding Blue-Green Deployment concepts