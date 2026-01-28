# 🏗️ Enterprise AWS VPC Architecture – Scalable & Secure Network

## 📌 Overview

This repository documents the design and implementation of a **production-ready AWS VPC architecture** built using **enterprise CIDR planning and routing best practices**.

The goal is to design the network **once** and allow it to scale for **years without redesign**, while enforcing **security by routing** and maintaining **audit readiness**.

---

## 🎯 Project Objectives

* Design a **single large VPC** with future growth in mind
* Implement **unequal subnet sizing** based on workload demand
* Enforce **controlled internet access**
* Ensure **private workloads remain fully isolated**
* Provide **clear validation, failure, and audit explanations**
* Produce **publish-ready documentation**

---

## 🧠 Business Scenario

The VPC acts as a **shared cloud network platform** hosting:

* Admin & operational services (Bastion / Ops)
* Internet-facing edge components (Ingress / Load Balancers)
* Web application tiers
* Application backends
* Platform & container workloads
* Large shared internal services

This architecture mirrors **real enterprise cloud foundations**.

---

## 🌐 Network Design Summary

### VPC

| Component   | Value                           |
| ----------- | ------------------------------- |
| CIDR        | `10.0.0.0/16`                   |
| Total IPs   | 65,536                          |
| Design Type | Enterprise / Long-term scalable |

---

### Subnet Design (Unequal Sizes)

| Subnet   | Purpose            | CIDR  | IP Capacity |
| -------- | ------------------ | ----- | ----------- |
| Shared   | Internal Services  | `/19` | ~8,192      |
| Platform | Containers / Tools | `/20` | ~4,096      |
| App      | Application Tier   | `/21` | ~2,048      |
| Web      | Web Tier           | `/22` | ~1,024      |
| Edge     | Ingress / LB       | `/23` | ~512        |
| Admin    | Bastion / Ops      | `/24` | ~256        |

✔ Largest subnets allocated first
✔ No CIDR overlap
✔ All CIDRs aligned correctly

---

## 🧭 Routing Architecture

### Route Tables

| Route Table | Subnets                    | Routes               |
| ----------- | -------------------------- | -------------------- |
| Public-RT   | Admin, Edge                | `0.0.0.0/0 → IGW`    |
| Private-RT  | Web, App, Platform, Shared | Local VPC route only |

📌 **No NAT Gateway is used** – private subnets are fully isolated by design.

---

## 🌍 Internet Access Model

* Only **Admin** and **Edge** subnets have internet access
* Private subnets have **no default route to IGW**
* Internal communication works via **local VPC routing**

This enforces **network-level security without relying on Security Groups**.

---

## 🧪 Validation & Testing

### Public Subnets

✔ Can access the internet
**Reason:** Route to Internet Gateway exists

### Private Subnets

❌ Cannot access the internet
**Reason:** No `0.0.0.0/0` route

### Internal Communication

✔ All subnets can communicate internally
**Reason:** Implicit local routing within the VPC

---

## 🚨 Failure & Audit Scenarios

### IGW Detached

* Internet access stops immediately
* Internal traffic remains unaffected

### Private Subnet Associated with Public Route Table

* Subnet becomes publicly reachable
* Major security & audit violation

### Incorrect CIDR Boundary

* Overlapping or misaligned CIDRs
* AWS rejects configuration or causes routing issues

---

## 📐 Architecture Diagram

The repository includes a **logical AWS VPC architecture diagram** showing:

* VPC boundary
* Public vs Private subnets
* Route tables
* Internet Gateway
* Traffic flow paths


---

## 📄 Documentation

Detailed documentation is provided covering:

* CIDR planning rationale
* Subnet calculations
* Routing behavior
* Validation tests
* Failure analysis
* Screenshot placeholders for audit evidence


---

## 🧩 Intended Audience

* Cloud / DevOps Engineers
* AWS Solution Architects
* Platform Engineering Teams
* Interview & Certification Preparation
* Enterprise Cloud Foundation Projects

---

## ✅ Key Takeaways

* Security enforced at **network level**
* Predictable, scalable CIDR design
* Clean separation of public & private workloads
* Audit-friendly and easy to reason about
* Production-ready by default

---

## 📜 License

This project is provided for **educational and reference purposes**.

