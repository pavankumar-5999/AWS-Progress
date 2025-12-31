# AWS Elastic Load Balancer (ELB) 

## What is ELB?

Elastic Load Balancer (ELB) is an AWS managed service that automatically distributes incoming traffic across multiple targets such as EC2 instances, containers, or IPs across multiple Availability Zones.

**Purpose:**

* High availability
* Fault tolerance
* Scalability
* Better performance

---

## Why ELB is used 

When millions of users access Netflix or Amazon simultaneously:

* ELB distributes traffic across many backend servers
* Prevents server overload
* Automatically redirects traffic if a server fails
* Works seamlessly with Auto Scaling during peak traffic

**Traffic flow:**

```
Users → ELB → Multiple EC2 instances → Application
```

---

## Types of ELB

1. **Application Load Balancer (ALB)** – Layer 7 (HTTP/HTTPS, path-based routing)
2. **Network Load Balancer (NLB)** – Layer 4 (TCP/UDP, ultra‑high performance)
3. **Gateway Load Balancer (GWLB)** – Security appliances
4. **Classic Load Balancer (CLB)** – Legacy (not recommended)

---

## What are Target Groups?

A Target Group defines **where the load balancer sends traffic**.

Target types:

* **Instance** – EC2 instances (most common)
* **IP** – Private IP addresses within the VPC
* **Lambda** – Serverless workloads

Target groups also define:

* Port & protocol
* Health check rules

---

## Which IP address is used in ELB?

### Golden Rule

* ❌ Never use public IP addresses
* ❌ Never use VPC CIDR blocks

### Correct usage

**Target type = Instance (Recommended)**

* Register EC2 instances
* ELB automatically uses **private IPs**

**Target type = IP**

* Manually enter **private IP addresses only**
* IPs must belong to the VPC CIDR range

Example:

```
VPC CIDR: 10.0.0.0/16
Valid IP: 10.0.1.15
Invalid: Public IP or 10.0.0.0/16
```

---

## Internet-facing vs Internal ELB

* **Internet-facing ELB** → Accessible from the internet
* **Internal ELB** → Used for private/internal applications

---

## Health Checks

* ELB continuously monitors target health
* Unhealthy targets are removed automatically
* Added back once healthy

---

## ELB with Auto Scaling

* Auto Scaling adds/removes EC2 instances
* ELB automatically updates target registration
* No manual work required

---

## SSL / HTTPS Support

* ELB supports HTTPS
* SSL certificates managed using **AWS Certificate Manager (ACM)**

---

## Best Practice Architecture

✔ Application Load Balancer
✔ EC2 instances in private subnets
✔ Auto Scaling enabled
✔ Target type = Instance

---
