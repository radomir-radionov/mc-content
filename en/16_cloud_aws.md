# Cloud services — AWS

Amazon Web Services — common services for Node backends.

[← Back to hub](./16_cloud.md)

### Related docs

[Cloud — Core](./16_cloud_core.md) · [Cloud — AWS](./16_cloud_aws.md) · [Cloud — GCP](./16_cloud_gcp.md) · [Cloud — Azure](./16_cloud_azure.md)

### Sections

- [AWS](#aws)

---

## AWS

<details>
  <summary>AWS</summary>

### AWS

**Definition:** Amazon Web Services — largest public cloud; hundreds of services. Backend teams usually start with compute, storage, database, and IAM.

**Why we need it:** Run production Node APIs without owning servers; use managed Postgres (RDS), file storage (S3), and queues (SQS) with SLAs and backups.

**When to use:**

| Service         | Use case                                                                           |
| --------------- | ---------------------------------------------------------------------------------- |
| **EC2**         | Full control — VMs, Docker, any stack                                              |
| **ECS / EKS**   | Docker containers at scale                                                         |
| **Lambda**      | Short serverless functions (webhooks, image resize)                                |
| **S3**          | Images, exports, static site assets                                                |
| **RDS**         | Managed Postgres/MySQL — backups, patches                                          |
| **ElastiCache** | Managed Redis                                                                      |
| **SQS**         | Simple message queue (like a basic [message broker](../12_message_broker_core.md)) |
| **CloudFront**  | CDN in front of S3 or API                                                          |
| **IAM**         | Users, roles, policies — who can call what                                         |

**Advantages:**

| Advantage             | What you gain                                |
| --------------------- | -------------------------------------------- |
| **Mature managed DB** | RDS snapshots, Multi-AZ failover             |
| **S3 durability**     | Standard for user uploads and backups        |
| **Huge ecosystem**    | Terraform modules, tutorials, hiring pool    |
| **Free tier**         | Small experiments on EC2, Lambda, RDS limits |

**Pluses and minuses:**

| +                           | −                                                |
| --------------------------- | ------------------------------------------------ |
| Most complete catalog       | Easy to misconfigure security groups / public S3 |
| MSK, SQS, SNS for messaging | Billing needs tags and Cost Explorer discipline  |
| Global regions              | IAM and VPC learning curve                       |

**Examples (conceptual stack):**

```
React on S3 + CloudFront
       ↓
ALB → ECS (Node API)
       ↓
RDS Postgres + ElastiCache Redis
       ↓
S3 (uploads) + SQS → Lambda/worker (email)
```

**Thoughts:** Prefer **IAM roles** on EC2/ECS over hard-coded access keys. S3 buckets: block public access by default. Compare queues: SQS ≈ simple queue; MSK ≈ Kafka.

**Sources:** [AWS documentation](https://docs.aws.amazon.com/) · [AWS Well-Architected](https://aws.amazon.com/architecture/well-architected/)

---

</details>
