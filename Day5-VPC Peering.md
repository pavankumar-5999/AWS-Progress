# AWS VPC Peering – Day 5 Notes

## 1. What is VPC Peering?

**VPC Peering** is a networking connection between two VPCs that enables routing of traffic using private IP addresses.

**Key points:**

* Traffic stays within AWS network (no internet traversal)
* VPCs can be in the same or different AWS regions (inter-region peering)
* Supports **one-to-one** VPC connectivity

---

## 2. Why is VPC Peering used?

* Share resources between VPCs (EC2, RDS, etc.)
* Connect development, staging, and production VPCs
* Enable communication across accounts securely
* Reduce data transfer over public internet

---

## 3. How VPC Peering works?

* Create a **peering connection** between two VPCs
* Update **route tables** in both VPCs to allow traffic
* Use **security groups** to allow communication
* **No transitive peering** (VPC A → VPC B → VPC C does not work directly)

---

## 4. Steps to create VPC Peering

1. Initiate a VPC peering request from one VPC (requester)
2. Accept the peering request in the other VPC (accepter)
3. Update route tables to allow traffic
4. Modify security groups if needed
5. Test connectivity

---

## 5. Common Questions & Answers

### Can VPC peering cross regions?

Yes, AWS supports **inter-region VPC peering**.

### Can VPC peering use public IPs?

No, communication uses **private IP addresses** only.

### Is VPC peering transitive?

No, peering is **one-to-one**. Traffic cannot route through a peered VPC to another VPC.

### Can VPC peering connect VPCs in different AWS accounts?

Yes, with proper permissions and acceptance of peering request.

### How is security handled?

Security groups and network ACLs still apply; peering does not bypass security rules.

### Can route tables be updated automatically?

No, route tables must be updated manually to direct traffic through the peering connection.

---

## 6. Best Practices

* Keep CIDR blocks **non-overlapping**
* Limit number of peering connections to avoid complexity
* Use **inter-region peering** only when needed
* Monitor using **VPC Flow Logs**
* Combine with **Transit Gateway** for large networks instead of multiple peering connections

---



<img width="1536" height="1024" alt="cb85ce53-a5d2-46ec-83df-ebef0c862747" src="https://github.com/user-attachments/assets/d9710a74-dd3d-4111-923d-ca2b463dd4f6" />


Route tables in both VPCs updated to allow traffic across the peering connection
Security groups allow necessary ports between EC2 instances

```
