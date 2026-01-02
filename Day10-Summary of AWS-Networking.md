# AWS Services Interview Summary 


## Day 1: AWS VPC (Virtual Private Cloud)

* Creates a **private network in AWS** where you launch resources.
* Supports **public and private subnets** for security and access control.
* **Route Tables** control traffic within the VPC and to the internet.
* **Internet Gateway** enables public subnet connectivity.
* CIDR blocks define the IP address range of the VPC.

**Interview Tip:** Know differences between **public vs private subnets**, route tables, and IGW.
---

## Day 2: NAT Gateway

* Allows instances in **private subnets** to access the internet **without exposing them publicly**.
* NAT Gateway is deployed in a **public subnet**.
* Associates with an **Elastic IP** for stable internet access.
* Supports **high availability** and scaling.
* Traffic flows: Private instance → NAT Gateway → Internet.

**Interview Tip:** Be clear on **why NAT Gateway is needed vs public subnet IGW**.

---

## Day 3: Firewall – Security Groups & NACLs

* **Security Groups:** Instance-level firewall, **stateful**, allow inbound/outbound rules.
* **Network ACLs (NACLs):** Subnet-level firewall, **stateless**, evaluate rules for inbound and outbound separately.
* Controls **traffic flow** in VPC.
* Security Groups are best for **instance-specific access**; NACLs for **broad subnet policies**.
* Helps enforce **least privilege principle**.

**Interview Tip:** Know **stateful vs stateless** differences and which is applied where.

---

## Day 4: AWS Elastic Load Balancer (ELB)

* Distributes incoming traffic across multiple **EC2 instances**.
* Types: **Application LB (Layer 7), Network LB (Layer 4), Gateway LB**.
* Supports **high availability and fault tolerance**.
* Can perform **health checks** to route traffic only to healthy instances.
* Integrates with **Auto Scaling** to handle traffic spikes.

**Interview Tip:** Understand **ALB vs NLB differences**, use cases, and health checks.

---

## Day 5: VPC Peering

* Connects **two VPCs privately** without using the internet.
* Supports **same or different AWS accounts**.
* Traffic uses **private IP addresses**.
* No overlapping CIDR blocks allowed.
* Improves **secure, low-latency communication** between VPCs.

**Interview Tip:** Know **VPC Peering vs Transit Gateway**, use cases.

---

## Day 6: AWS VPN & Direct Connect

### VPN

* **Region-based**, secure connection over the internet.
* Uses **encrypted tunnels**.
* Quick to set up, lower cost, ideal for small-medium setups.
* Performance depends on internet ISP.

### Direct Connect

* **Global service** providing dedicated private line to AWS.
* Stable, predictable performance and low latency.
* Takes time to set up, higher cost.
* Connects multiple AWS Regions if needed.

**Interview Tip:** Be able to **compare VPN vs Direct Connect** and usage scenarios.

---

## Day 7: AWS CloudFront

* **Global CDN** for fast content delivery.
* Caches content near users (edge locations).
* Works with S3, EC2, ALB.
* TTL (Time to Live) controls cache duration; default **24 hours (86400 sec)**.
* Supports **cache invalidation** to remove content immediately.

**Interview Tip:** Remember **CloudFront = caching content globally** vs **Global Accelerator = traffic acceleration**.

---

## Day 8: AWS Global Accelerator

* **Global service** improving application speed and availability.
* Uses **AWS private global network**.
* Provides **two static anycast IPs** for your app.
* Routes traffic to **nearest healthy endpoint**.
* Works with apps in multiple regions.

**Interview Tip:** Difference between **Global Accelerator vs CloudFront vs Route 53**.

---

## Day 9: AWS Route 53

* **Global DNS web service**.
* Maps domain names to AWS resources (EC2, S3, ALB).
* Supports health checks, failover, and multiple routing policies.
* Ensures users reach the **closest and healthy endpoint**.
* Can register domain names directly.

**Interview Tip:** Remember **Route 53 = internet phone book**; understand **routing policies**.

---

* **Scope (Regional vs Global)**
* **Use cases**
* **Key components and terminology**
* **Comparison between similar services (VPN vs Direct Connect, CloudFront vs Global Accelerator)**
* **Traffic flow examples**


