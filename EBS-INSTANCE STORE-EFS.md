# AWS Storage Cheat Sheet

---

## 1️⃣ High‑Level Comparison (Most Important)

| Feature                | **EBS**         | **EC2 Instance Store** | **EFS**           |
| ---------------------- | --------------- | ---------------------- | ----------------- |
| Storage Type           | Block           | Block                  | File (NFS)        |
| Persistence            | ✅ Yes           | ❌ No (ephemeral)       | ✅ Yes             |
| Scope                  | AZ              | EC2 host               | Region (Multi‑AZ) |
| Attach to Multiple EC2 | ❌ No*           | ❌ No                   | ✅ Yes             |
| Data Lost on Stop      | ❌ No            | ✅ Yes                  | ❌ No              |
| Typical Latency        | Low             | Very Low               | Medium            |
| OS Support             | Linux / Windows | Linux / Windows        | Linux only        |
| Common Use             | OS, DB, Apps    | Cache, temp data       | Shared files      |

* EBS Multi‑Attach only for io1/io2 (rare exam case)

---

## 2️⃣ EBS – Volume Types (Performance Models)

| EBS Type      | Performance Model    | Key Trait              | Real‑World Use            |
| ------------- | -------------------- | ---------------------- | ------------------------- |
| **gp2 / gp3** | Burstable            | Cost‑effective         | App servers, dev/test     |
| **io1 / io2** | Provisioned IOPS     | Guaranteed low latency | Databases (Oracle, MySQL) |
| **st1**       | Throughput Optimized | Sequential I/O         | Logs, big data            |
| **sc1**       | Cold HDD             | Cheapest               | Infrequent access         |

**Exam Rules**

* "Consistent IOPS" → io1 / io2
* "Cheap general storage" → gp3
* "Sequential throughput" → st1

---

## 3️⃣ EC2 Instance Store – When to Use

| Aspect      | Value             |
| ----------- | ----------------- |
| Persistence | ❌ None            |
| Performance | 🚀 Highest IOPS   |
| Backup      | ❌ Not possible    |
| Cost        | Included with EC2 |

**Real‑World Scenarios**

* Redis / Memcached
* Temporary rendering files
* Scratch space

**Exam Keyword**

> "Data can be lost" + "very high performance"

---

## 4️⃣ EFS – Modes Breakdown (Common Exam Trap)

### A. Performance Modes (Latency & Concurrency)

| Mode                | What It Optimizes   | Use Case            |
| ------------------- | ------------------- | ------------------- |
| **General Purpose** | Low latency         | Web apps, CMS       |
| **Max I/O**         | Massive concurrency | Big data, analytics |

---

### B. Throughput Modes (This is where **Elastic** lives)

| Mode            | Throughput Behavior | When to Choose                |
| --------------- | ------------------- | ----------------------------- |
| **Bursting**    | Scales with size    | Default workloads             |
| **Provisioned** | Fixed throughput    | Small FS, high constant load  |
| **Elastic**     | Auto‑scales         | Spiky & unpredictable traffic |

**Keywords**

* "Unpredictable" / "No capacity planning" → **Elastic**
* "Guaranteed throughput" → Provisioned

---

## 5️⃣ Decision Table (Fast Elimination)

| Question                              | Correct Answer |
| ------------------------------------- | -------------- |
| Persistent storage for single EC2     | EBS            |
| Extremely fast but disposable storage | Instance Store |
| Shared storage across EC2s            | EFS            |
| Database with low latency             | EBS io1/io2    |
| Unpredictable shared workload         | EFS Elastic    |

---

## 6️⃣ Brutal Memory Hack

* **One EC2 → EBS**
* **Many EC2 → EFS**
* **Can lose data → Instance Store**
