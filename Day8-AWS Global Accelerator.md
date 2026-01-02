# AWS Global Accelerator
---

## What is AWS Global Accelerator?

AWS Global Accelerator is a service that **improves the speed and reliability of your application for users around the world**.

It does this by directing user traffic through **AWS’s global network**, instead of the normal public internet.

---

## Simple Analogy

Think of AWS Global Accelerator like this:

> You have a call center in one city, but customers are calling from all over the world. Instead of everyone using slow public phone lines, AWS gives you **fast private international lines** that connect callers to the nearest entry point and then quickly route them to your call center.

Users reach AWS faster, and AWS carries the traffic safely and quickly inside its own network.

---

## Simple Example

* Your application is running on AWS (EC2, ALB, or NLB)
* Users access it from different countries
* Global Accelerator gives your app **two static IP addresses**
* Users connect to the **nearest AWS location**
* AWS carries the traffic internally to your application

### Real-World Example (Amazon Websites)

* A customer opens **amazon.in** from India
* Another customer opens **amazon.co.uk** from the UK
* Another customer opens **amazon.com** from the US

Even though the website looks the same:

* Each user connects to the **nearest AWS edge location**
* AWS Global Accelerator routes traffic through its **private global network**
* Traffic is sent to the closest healthy backend (Region)

Result: **Faster response and better availability**.

---

## Is AWS Global Accelerator Global or Regional?

**AWS Global Accelerator is a Global service.**

What this means:

* It works across the entire world
* Uses AWS global edge locations
* Improves access to applications hosted in any AWS Region

Simple memory line:

> **Global Accelerator is global, but your application still runs in a Region.**

---

## Why Do We Use AWS Global Accelerator?

* Faster access for global users
* Lower latency
* Improved application availability
* Automatic routing around failures

---

## What Makes Global Accelerator Different?

* Uses **AWS private global network** (not the public internet)
* Provides **static IP addresses**
* Automatically routes traffic to the **best healthy endpoint**

---

## AWS Global Accelerator 

* Global service that improves application speed
* Uses AWS private global network
* Provides two static anycast IPs
* Routes users to nearest AWS entry point
* Improves performance and availability

---

> **CloudFront = Caches content**
> **Global Accelerator = Accelerates network traffic**

---
