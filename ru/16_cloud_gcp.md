# Cloud services — GCP

Google Cloud Platform — compute, data, and serverless for backends.

[← Back to hub](./16_cloud.md)

### Related docs

[Cloud — Core](./16_cloud_core.md) · [Cloud — AWS](./16_cloud_aws.md) · [Cloud — GCP](./16_cloud_gcp.md) · [Cloud — Azure](./16_cloud_azure.md)

### Sections

- [GCP](#gcp)

---

## GCP

<details>
  <summary>GCP</summary>

### GCP

**Definition:** Google Cloud Platform — Google’s public cloud. Strong in networking, Kubernetes (GKE), data analytics (BigQuery), and serverless containers (Cloud Run).

**Why we need it:** Same benefits as AWS — managed infra — with opinionated paths for containers and data pipelines if you already use Google tooling or BigQuery.

**When to use:**

| Service | Use case |
| ------- | -------- |
| **Compute Engine** | VMs (like EC2) |
| **Cloud Run** | Containerized HTTP services — scale to zero, simple deploy |
| **GKE** | Managed Kubernetes |
| **Cloud Storage** | Object storage (like S3) |
| **Cloud SQL** | Managed Postgres/MySQL |
| **Memorystore** | Managed Redis |
| **Pub/Sub** | Async messaging, event fan-out |
| **Cloud Functions** | Serverless functions |
| **BigQuery** | Serverless data warehouse / analytics |
| **Cloud IAM** | Access control |

**Advantages:**

| Advantage | What you gain |
| --------- | ------------- |
| **Cloud Run simplicity** | Deploy container from Dockerfile — minimal ops |
| **Global network** | Low-latency backbone between regions |
| **BigQuery** | Huge SQL analytics without managing clusters |
| **GKE** | Reference Kubernetes experience |

**Pluses and minuses:**

| + | − |
| --- | --- |
| Excellent for data/ML pipelines | Smaller third-party tutorial pool than AWS in some niches |
| Cloud Run DX for Node APIs | Fewer enterprise features than Azure in some accounts |
| Per-second billing on many services | IAM + org/folder hierarchy takes time to learn |

**Examples (conceptual stack):**

```
Frontend on Firebase Hosting or Cloud Storage + CDN
       ↓
Cloud Run (Node API)
       ↓
Cloud SQL Postgres + Memorystore Redis
       ↓
Pub/Sub → Cloud Run worker (background jobs)
       ↓
BigQuery (analytics from streamed events)
```

**Thoughts:** **Cloud Run** is a sweet spot for Node APIs that want containers without managing Kubernetes. Pub/Sub maps to async patterns in [message broker](../12_message_broker_core.md) docs.

**Sources:** [Google Cloud documentation](https://cloud.google.com/docs)

---

</details>
