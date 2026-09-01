# 01 — Cloud Computing Basics & AWS Global Infrastructure

Quick revision notes.

## What is cloud computing?

On-demand delivery of IT resources (compute, storage, database, network, software) over the internet, with pay-as-you-go pricing. No buying or maintaining physical hardware.

**6 advantages of cloud (AWS official list):**

1. Trade capital expense (CapEx) for variable expense (OpEx)
2. Benefit from massive economies of scale
3. Stop guessing capacity
4. Increase speed and agility
5. Stop spending money running and maintaining data centers
6. Go global in minutes

## Service models

| Model | You manage | Provider manages | AWS example |
|---|---|---|---|
| **IaaS** | OS, runtime, app, data | Hardware, network, virtualisation | Amazon EC2, EBS, VPC |
| **PaaS** | App code + data | OS, runtime, scaling, LB | AWS Elastic Beanstalk, Lambda, RDS |
| **SaaS** | Just your data/settings | Everything | Amazon WorkMail, WorkDocs, Chime |

Memory hook: **land & bricks → ready building → furnished flat**.

## Deployment models

| Model | Meaning | When to use |
|---|---|---|
| **Public cloud** | Everything runs on the provider's infra (AWS) | Startups, web apps, most new workloads |
| **Private cloud / on-premises** | Own data center | Strict regulation, legacy systems |
| **Hybrid cloud** | On-prem + public cloud connected together | Critical data stays in-house, cloud bursting for peaks |

**Cloud bursting** = keep the base load on-premises, push the overflow to AWS during spikes.

## AWS Global Infrastructure

```
Region  →  Availability Zones  →  Data Centers
Edge Locations (separate, much larger network)  →  End Users
```

- **Region** — geographic area (Mumbai `ap-south-1`, N. Virginia `us-east-1`). Choose based on **latency, compliance/data residency, service availability, price**.
- **Availability Zone (AZ)** — one or more discrete data centers with independent power, cooling and networking. Named `ap-south-1a`, `ap-south-1b`, … Minimum 2 per Region (usually 3+). Connected by low-latency private fibre.
- **Data Center** — the actual physical building with the servers.
- **Edge Location** — CloudFront CDN cache points in many more cities than Regions. Also used by Route 53, AWS Global Accelerator, AWS Shield and WAF.
- **Local Zones / Wavelength Zones** — extensions of a Region for ultra-low latency in specific cities / on 5G networks.

### Key rule

> Deploy across **multiple AZs** for high availability inside a Region.
> Deploy across **multiple Regions** for disaster recovery and global latency.

## High-availability building blocks

| Need | AWS service |
|---|---|
| Spread traffic across AZs | Elastic Load Balancer (ALB/NLB) |
| Keep instance count healthy | EC2 Auto Scaling group (multi-AZ) |
| Database failover | Amazon RDS Multi-AZ / Aurora |
| Durable object storage | Amazon S3 (multi-AZ by default) |
| Session state outside the server | ElastiCache / DynamoDB |
| DNS failover & routing | Amazon Route 53 health checks |
| Global caching | Amazon CloudFront |

## Terms to remember

- **Elasticity** — scale up/down automatically with demand.
- **Scalability** — ability to grow (vertical = bigger instance, horizontal = more instances).
- **High Availability (HA)** — stays up during a failure (multi-AZ).
- **Fault tolerance** — no impact at all during a failure (redundant components).
- **Disaster Recovery (DR)** — recover in another Region; measured by **RTO** (time to recover) and **RPO** (acceptable data loss).
- **Shared Responsibility Model** — AWS is responsible for security **of** the cloud; the customer is responsible for security **in** the cloud.
