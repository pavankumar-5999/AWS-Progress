# AWS EC2 Purchasing Options – Exam Cram Cheat Sheet

## Core Comparison Table

| Option                            | Commitment | Discount   | Interruptible | Capacity Reserved | Flexibility | Exam Keywords                                   | AWS Wants This When                 |
| --------------------------------- | ---------- | ---------- | ------------- | ----------------- | ----------- | ----------------------------------------------- | ----------------------------------- |
| **On-Demand**                     | None       | 0%         | No            | No                | High        | no commitment, pay per second                   | Short-term, unpredictable workloads |
| **Standard Reserved Instance**    | 1–3 years  | Up to ~72% | No            | Yes (Zonal only)  | Low         | predictable, steady state, capacity reservation | Fixed long-running workloads        |
| **Convertible Reserved Instance** | 1–3 years  | Up to ~54% | No            | No                | Medium      | change instance family                          | Workloads with future uncertainty   |
| **EC2 Instance Savings Plan**     | 1–3 years  | Up to ~72% | No            | No                | Medium      | instance family + region                        | Stable EC2-only usage               |
| **Compute Savings Plan**          | 1–3 years  | Up to ~66% | No            | No                | Very High   | most flexible, across services                  | Default long-term cost optimization |
| **Spot Instances**                | None       | Up to ~90% | Yes           | No                | High        | fault-tolerant, 2-minute warning                | Batch, async, CI/CD, big data       |
| **Spot Fleet (Legacy)**           | None       | Up to ~90% | Yes           | No                | Low         | legacy, multiple Spot pools                     | Older architectures only            |
| **Dedicated Instances**           | None       | 0%         | No            | Yes               | Low         | hardware isolation, no host control             | Compliance requirements             |
| **Dedicated Hosts**               | Host-level | 0%         | No            | Yes               | Very Low    | BYOL, socket/core visibility                    | Licensing constraints               |

---

## Exam Trap Keywords → Correct Choice

| Question Phrase            | Meaning                  | Correct Answer                |
| -------------------------- | ------------------------ | ----------------------------- |
| lowest possible cost       | use spare capacity       | Spot Instances                |
| must not be interrupted    | cannot terminate         | On-Demand / RI / Savings Plan |
| predictable workload       | steady long-term usage   | Savings Plan or RI            |
| need flexibility           | avoid instance lock-in   | Compute Savings Plan          |
| license tied to cores      | physical hardware needed | Dedicated Hosts               |
| compliance isolation       | no shared hardware       | Dedicated Instances           |
| instance family may change | future uncertainty       | Convertible RI                |

---

## RI vs Savings Plan – Exam Snapshot

| Clue in Question               | Choose                  |
| ------------------------------ | ----------------------- |
| mentions instance type         | Reserved Instance       |
| mentions $ per hour commitment | Savings Plan            |
| mentions Lambda or Fargate     | Compute Savings Plan    |
| mentions capacity reservation  | Zonal Reserved Instance |

---

## Absolute Exam Rules (Memorize)

* Savings Plans provide **pricing discounts only**, not capacity
* Reserved Instances do **not** reserve capacity unless zonal
* Spot Instances are **always interruptible**
* Dedicated Hosts are for **licensing requirements**
* Dedicated Instances are **not the same** as Dedicated Hosts
* Spot Fleet is **legacy** and not recommended for new designs

---

**Exam Default:** If no strong constraints are given and the question asks for long-term cost optimization → **Compute Savings Plan**
