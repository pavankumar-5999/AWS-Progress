## What is a Firewall?

A **firewall** is a **security system** that monitors and controls **incoming and outgoing network traffic** based on predefined rules.

In simple words:

> **A firewall decides what traffic is allowed and what traffic is blocked.**

It acts like a **security guard** between a trusted network and an untrusted network (like the internet).

## 1. What is Stateful?

A **stateful** firewall remembers the state of a connection.

Meaning:

* If you allow **inbound traffic**, the **outbound response is automatically allowed**
* You don’t need to create separate rules for return traffic

Example:

* Allow inbound HTTP (port 80)
* Response traffic is allowed automatically

---

## 2. What is Stateless?

A **stateless** firewall does NOT remember the state of a connection.

Meaning:

* You must explicitly allow **both inbound and outbound traffic**
* If one direction is missing, traffic is blocked

Example:

* Allow inbound HTTP (port 80)
* You must also allow outbound traffic manually

---

## 3. What is a Security Group?

A **Security Group** is a **virtual firewall for EC2 instances and other AWS resources**.

### Where is it configured?

* Configured at the **instance level** (EC2, ALB, RDS, etc.)

### Key Features

* Controls inbound and outbound traffic
* Only **ALLOW rules** are supported
* Default behavior: Deny all inbound, allow all outbound

### Is Security Group Stateful or Stateless?

➡️ **Security Groups are STATEFUL**

### Example

* Inbound: Allow HTTP (80) from anywhere
* Outbound: Automatically allowed

If a user accesses your EC2 on port 80, the response is allowed automatically.

---

## 4. What is a NACL (Network Access Control List)?

A **NACL** is a firewall that controls traffic **at the subnet level**.

### Where is it configured?

* Configured at the **subnet level**

### Key Features

* Controls inbound and outbound traffic for subnets
* Supports **ALLOW and DENY rules**
* Rules are evaluated in **number order (lowest first)**

### Is NACL Stateful or Stateless?

➡️ **NACLs are STATELESS**

### Example

* Inbound: Allow HTTP (80)
* Outbound: Must explicitly allow HTTP response

If outbound rule is missing, traffic will be blocked.

---

## 5. Difference Between Security Group and NACL

| Feature    | Security Group | NACL               |
| ---------- | -------------- | ------------------ |
| Level      | Instance level | Subnet level       |
| Type       | Stateful       | Stateless          |
| Rules      | Allow only     | Allow & Deny       |
| Rule Order | Not applicable | Evaluated in order |
| Default    | Deny inbound   | Allow all          |

---

## 6. Default Security Group and Default NACL

### Default Security Group

* Allows inbound traffic from **same security group**
* Allows all outbound traffic

### Default NACL

* Allows all inbound traffic
* Allows all outbound traffic

---

## 7. How Security Group and NACL Work Together

Traffic must pass **both**:

1. NACL rules (subnet level)
2. Security Group rules (instance level)

If either blocks traffic → **request fails**.

---

# 🚀 Day 3 Cheat Sheet (Easy to Remember)

* **Stateful** → Response allowed automatically
* **Stateless** → Allow both directions manually
* **Security Group** → Instance-level firewall (Stateful)
* **NACL** → Subnet-level firewall (Stateless)

### One-Line Memory Trick

> **NACL checks first (Subnet) → Security Group checks next (Instance)**

### Super Memory Hack

* **S**ecurity **G**roup → **S**tateful
* **N**ACL → **N**ot stateful (Stateless)
