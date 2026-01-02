# AWS Route 53 

## What is AWS Route 53?

AWS Route 53 is a **Domain Name System (DNS) web service** that **helps users find your website or application on the internet**.

Think of it like this:

> You have a friend named “Alice.” Instead of remembering her phone number, you just remember her name. The phone book tells you the number. Route 53 is like that **internet phone book** — it translates a website name into the IP address that computers use to connect.

---

## Simple Analogy

* **Website URL (example.com) = Friend’s name (Alice)**
* **IP address = Friend’s phone number**
* **Route 53 = Phone book that maps name to number**

When someone types **[www.example.com](http://www.example.com)**, Route 53 tells their computer which IP address to contact so they reach your website.

---

## Simple Example

* Your website is hosted on AWS (EC2, S3, ALB)
* Users type **[www.mysite.com](http://www.mysite.com)**
* Route 53 looks up the IP address
* Sends users to the correct AWS resource

Result: Users reach your website **quickly and reliably**.

---

## Is AWS Route 53 Global or Regional?

**AWS Route 53 is a Global service.**

What this means:

* DNS works globally
* Users anywhere in the world can resolve your domain
* You can set up **traffic routing policies** to different AWS Regions

Simple memory line:

> **Route 53 is global, but can route users to resources in specific Regions.**

---

## Why Do We Use AWS Route 53?

* Map domain names to AWS resources
* Route users to healthy endpoints
* Load balancing across Regions
* Domain registration and management

---

## Key Concepts (Beginner-Friendly)

### 1. Domain Name

The name people type in the browser, e.g., **[www.example.com](http://www.example.com)**

### 2. Record Set

Rules that tell Route 53 where to send traffic for a domain or subdomain.

* Example: [www.example.com](http://www.example.com) → 192.0.2.1

### 3. TTL (Time To Live)

* How long DNS information is cached by users or other DNS servers
* After TTL expires, Route 53 updates records if changed

### 4. Health Check

* Route 53 can **monitor your resources**
* If a server fails, traffic is routed to a healthy one

### 5. Routing Policies

* Simple: Direct all traffic to one resource
* Weighted: Split traffic between resources
* Latency-based: Send users to the fastest Region
* Failover: Route traffic to backup if primary fails

---

## AWS Route 53
* Global DNS web service
* Maps domain names to AWS resources
* Supports health checks and failover
* Offers multiple routing policies (simple, weighted, latency-based)
* Ensures fast, reliable user access worldwide

---
> **Route 53 = Internet phone book**
> **Domain name = Friend’s name**
> **IP address = Friend’s phone number**
> **Routing policies = Rules for choosing which friend’s number to call**

---

