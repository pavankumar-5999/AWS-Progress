## 1. What is a NAT Gateway?

A **NAT Gateway (Network Address Translation Gateway)** is an AWS-managed service that allows **instances in a private subnet to access the internet** without allowing inbound internet traffic.

In simple words:

> **Private servers can go out to the internet, but the internet cannot come in.**

---

## 2. Why Do We Need a NAT Gateway?

We need a NAT Gateway when:

* Private instances need software updates
* Backend servers need to access APIs
* Databases need outbound internet access

Reasons:

* Keeps private subnets secure
* Prevents direct internet exposure
* Allows outbound-only communication

---

## 3. How Does a NAT Gateway Work?

Step-by-step flow:

1. Private instance sends request to the internet
2. Route table sends traffic to NAT Gateway
3. NAT Gateway replaces private IP with public IP (Elastic IP)
4. Internet sends response back to NAT Gateway
5. NAT Gateway forwards response to private instance

➡️ The private IP is **never exposed** to the internet.

---

## 4. Where Should NAT Gateway Be Deployed?

A NAT Gateway **must be deployed in a Public Subnet**.

Why?

* NAT Gateway needs internet access
* Public subnet has a route to Internet Gateway (IGW)

Private subnet → Route to NAT Gateway → Public Subnet → IGW → Internet

---

## 5. What is an Elastic IP?

An **Elastic IP (EIP)** is a **static public IPv4 address** provided by AWS.

Key points:

* Remains the same even if resources restart
* Can be attached/detached
* Used when a fixed public IP is required

---

## 6. Link Between Elastic IP and NAT Gateway

The NAT Gateway **uses an Elastic IP** to communicate with the internet.

Relationship:

* NAT Gateway requires one Elastic IP
* Elastic IP provides a public identity
* All outbound traffic from private subnet appears to come from this Elastic IP

Without Elastic IP, NAT Gateway cannot access the internet.

---

## 7. Additional Important Concepts (You Should Know)

### a. NAT Gateway vs NAT Instance

* **NAT Gateway** → Managed, scalable, highly available
* **NAT Instance** → EC2-based, manual scaling, needs maintenance

➡️ NAT Gateway is recommended by AWS.

### b. High Availability

* NAT Gateway is AZ-specific
* Best practice: Create one NAT Gateway per Availability Zone

### c. Cost Consideration

* NAT Gateway is a paid service
* Charged per hour + data processed

---

# 🚀 Day 2 Cheat Sheet (Easy to Remember)

* **NAT Gateway** → Internet access for private subnet
* **Private Subnet** → No direct internet
* **Public Subnet** → Has IGW
* **Elastic IP** → Static public IP
* **NAT + EIP** → Safe outbound internet



> **Private needs internet → NAT Gateway → Public Subnet → IGW → Internet**
