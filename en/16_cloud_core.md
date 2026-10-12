# Cloud services — Core

Cloud fundamentals and how to choose AWS, GCP, or Azure.

[← Back to hub](./16_cloud.md)

### Related docs

[Cloud — Core](./16_cloud_core.md) · [Cloud — AWS](./16_cloud_aws.md) · [Cloud — GCP](./16_cloud_gcp.md) · [Cloud — Azure](./16_cloud_azure.md)

### Sections

- [Cloud foundations](#cloud-foundations)

---

## Cloud foundations

<details>
  <summary>Cloud computing</summary>

### Cloud computing

**Definition:** Running apps on someone else’s data centers over the internet — you rent compute, storage, and managed services instead of buying physical servers.

**Why we need it:** Start small (one small VM), scale up for traffic spikes, pay for what you use, get managed databases and queues without hiring a datacenter team.

**How it works:**

| Layer                                     | You manage       | Provider manages              |
| ----------------------------------------- | ---------------- | ----------------------------- |
| **IaaS** (EC2, Compute Engine)            | OS, runtime, app | Hardware, network, hypervisor |
| **PaaS** (Heroku, Cloud Run, App Service) | App code, config | Runtime, scaling, patching    |
| **SaaS** (SendGrid, Stripe dashboard)     | Usage            | Entire product                |

**When to use:** Production hosting, managed Postgres, object storage for uploads, background jobs, CDN — almost every deployed backend.

**Advantages:**

| Advantage            | What you gain                                              |
| -------------------- | ---------------------------------------------------------- |
| **Elastic scale**    | Add instances or serverless concurrency when traffic grows |
| **Managed services** | RDS / Cloud SQL instead of hand-tuning Postgres on a VM    |
| **Global regions**   | Deploy closer to users (EU, US, Asia)                      |
| **Pay-as-you-go**    | No upfront hardware; watch costs with budgets and alerts   |

**Pluses and minuses:**

| +                                     | −                                       |
| ------------------------------------- | --------------------------------------- |
| Fast to provision resources           | Bill surprise if untagged / unmonitored |
| High availability options (multi-AZ)  | Vendor lock-in on proprietary APIs      |
| Rich ecosystem (IAM, monitoring, CDN) | Learning curve for IAM and networking   |

**Thoughts:** Default for new backends: **managed database + object storage + one compute option** (containers or serverless). Run `terraform` or IaC when the stack grows.

**Sources:** [AWS — What is cloud computing?](https://aws.amazon.com/what-is-cloud-computing/) · [Google Cloud overview](https://cloud.google.com/docs/get-started)

---

</details>

<details>
  <summary>Regions, availability, and shared responsibility</summary>

### Regions, availability, and shared responsibility

**Definition:**

| Term                       | Meaning                                                                                      |
| -------------------------- | -------------------------------------------------------------------------------------------- |
| **Region**                 | Geographic area (e.g. `eu-west-1`, `europe-west1`) — pick for latency and data residency     |
| **Availability zone (AZ)** | Isolated datacenter within a region — spread replicas across AZs for HA                      |
| **Shared responsibility**  | Provider secures **the cloud**; you secure **in the cloud** (app code, IAM keys, open ports) |

**When to use:** Always choose region near users or where law requires data (GDPR). Run DB with multi-AZ / HA in production.

**Thoughts:** Leaked `.env` with root keys is your bug, not the cloud’s. Use least-privilege IAM roles, not long-lived root credentials.

**Sources:** [AWS shared responsibility](https://aws.amazon.com/compliance/shared-responsibility-model/)

---

</details>

<details>
  <summary>AWS vs GCP vs Azure</summary>

### AWS vs GCP vs Azure

**Definition:** Three major hyperscalers — same ideas, different product names.

| Category                 | AWS            | GCP                | Azure                       |
| ------------------------ | -------------- | ------------------ | --------------------------- |
| **Virtual machines**     | EC2            | Compute Engine     | Virtual Machines            |
| **Object storage**       | S3             | Cloud Storage      | Blob Storage                |
| **Managed SQL**          | RDS (Postgres) | Cloud SQL          | Azure SQL / PostgreSQL      |
| **Serverless functions** | Lambda         | Cloud Functions    | Azure Functions             |
| **Containers**           | ECS / EKS      | Cloud Run / GKE    | Container Apps / AKS        |
| **Message queue**        | SQS            | Pub/Sub            | Service Bus / Queue Storage |
| **Managed Kafka**        | MSK            | Pub/Sub / Dataflow | Event Hubs                  |
| **CDN**                  | CloudFront     | Cloud CDN          | Azure CDN                   |
| **Identity**             | IAM            | Cloud IAM          | Microsoft Entra ID          |

**When to use:**

| Pick      | Typical reason                                                             |
| --------- | -------------------------------------------------------------------------- |
| **AWS**   | Largest service catalog, most jobs list AWS, default for many startups     |
| **GCP**   | Strong data/ML (BigQuery), Kubernetes (GKE), good DX for Cloud Run         |
| **Azure** | Enterprise Microsoft stack, .NET-heavy teams, hybrid with Active Directory |

**Pluses and minuses:**

| Provider  | Pluses                               | Minuses                                     |
| --------- | ------------------------------------ | ------------------------------------------- |
| **AWS**   | Most services, docs, community       | Console complexity, easy to over-provision  |
| **GCP**   | Networking, data analytics, GKE      | Smaller job market than AWS in some regions |
| **Azure** | Enterprise contracts, AD integration | Portal UX, naming inconsistency             |

**Thoughts:** Skills transfer — S3 ≈ Cloud Storage ≈ Blob; Lambda ≈ Cloud Functions ≈ Azure Functions. Learn **concepts** once, map names per provider. Deep dives: [AWS](./16_cloud_aws.md) · [GCP](./16_cloud_gcp.md) · [Azure](./16_cloud_azure.md).

**Sources:** [AWS products](https://aws.amazon.com/products/) · [GCP products](https://cloud.google.com/products) · [Azure products](https://azure.microsoft.com/products)

---

</details>

<details>
  <summary>Simple full-stack on cloud</summary>

### Simple full-stack on cloud

**Definition:** Where each layer of a typical web app often lives in the cloud.

| Layer           | Job                      | Example (any provider)                                                           |
| --------------- | ------------------------ | -------------------------------------------------------------------------------- |
| **Frontend**    | Static SPA or SSR        | S3 + CloudFront / Firebase Hosting / Azure Static Web Apps                       |
| **Backend API** | Node API                 | EC2, Cloud Run, App Service, or Vercel/Railway (also cloud)                      |
| **Database**    | Postgres source of truth | RDS, Cloud SQL, Azure Database for PostgreSQL                                    |
| **Cache**       | Sessions, hot reads      | ElastiCache (Redis), Memorystore, Azure Cache for Redis                          |
| **Files**       | User uploads             | S3, Cloud Storage, Blob Storage                                                  |
| **Queue**       | Background email         | SQS, Pub/Sub, Service Bus — or [RabbitMQ/Kafka](../12_message_broker.md) on a VM |
| **Analytics**   | Dashboards on events     | ClickHouse Cloud, BigQuery, or sync to [ClickHouse](./9_database_clickhouse.md)  |

```
User → CDN → Frontend (static)
              ↓
         Backend API (compute)
         ↙    ↓    ↘
    Postgres  Redis  S3
         ↓
      SQS → worker → SendGrid
```

**Thoughts:** You do not need every service on day one. MVP: **compute + managed Postgres + S3-like storage**. Add Redis and queue when traffic or async work appears.

**Sources:** [AWS architecture center](https://aws.amazon.com/architecture/)

---

</details>
