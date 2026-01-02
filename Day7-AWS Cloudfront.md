# AWS CloudFront 

This document explains **AWS CloudFront** in **very simple terms**, using **analogies and real-life examples**, avoiding heavy jargon. It also explains **scope (Global vs Region)** and key components like **Distribution, Regions, TTL, Default TTL, and Cache Invalidation**. Perfect for beginners and GitHub notes.

---

## What is AWS CloudFront?

AWS CloudFront is a **content delivery service** that helps deliver websites, images, videos, and APIs **faster to users all around the world**.

### Simple Analogy

Think of CloudFront like this:

> You run a popular restaurant (your website). Instead of everyone traveling to one city to eat, you open **small branches near customers worldwide** so they get food faster.

CloudFront places copies of your content closer to users so they don’t have to wait.

---

## Simple Example

* Your website is hosted on an AWS server (like S3 or EC2)
* Users access it from different countries
* CloudFront stores copies of your content closer to users
* Users get the content faster with less delay

---

## Is AWS CloudFront Regional or Global?

**AWS CloudFront is a Global service.**

What this means:

* CloudFront works across the world
* It uses many global edge locations
* Users are served from the nearest location

Simple memory line:

> **CloudFront is global, content is delivered from the nearest location.**

---

## Key CloudFront Components

### 1. Region 

Your **original content** (origin) lives in a **Region**.

Examples:

* S3 bucket in ap-south-1
* EC2 server in us-east-1

CloudFront fetches content from this Region only when needed.

---

### 2. Distribution

A **CloudFront Distribution** is the setup that tells CloudFront:

* Where your original content lives (origin)
* How content should be cached
* Who can access it

Think of a distribution as:

> **A rulebook CloudFront follows to deliver your content.**

---

### 3. TTL (Time To Live)

**TTL** defines **how long CloudFront keeps a cached copy** before checking for updates.

Analogy:

> Like keeping food warm for a fixed time before cooking fresh again.

If TTL is active, CloudFront serves cached content without going back to origin.

---

### 4. Default TTL

The **default TTL is 24 hours (86400 seconds)**.

Meaning:

* CloudFront keeps content cached for 24 hours
* After that, it checks the origin for updates

You can increase or decrease this value.

---

### 5. Cache Invalidation

**Cache Invalidation** forces CloudFront to **remove cached content immediately**.

Use case:

* You updated a file
* You don’t want to wait for TTL to expire

Analogy:

> Like throwing away old food immediately instead of waiting.

---

## Why Do We Use AWS CloudFront?

* Faster website loading
* Reduced load on origin servers
* Better user experience
* Global content delivery

---

## AWS CloudFront
* Global content delivery service
* Caches content near users
* Works with S3, EC2, ALB, APIs
* Uses TTL to control caching
* Improves speed and performance

---

* **CloudFront = Global CDN**
* **Distribution = Delivery rulebook**
* **TTL = Cache time**
* **Default TTL = 24 hours (86400 sec)**
* **Invalidation = Clear cache now**

