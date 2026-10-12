# Cloud services — Azure

Microsoft Azure — VMs, storage, and enterprise integration.

[← Back to hub](./16_cloud.md)

### Related docs

[Cloud — Core](./16_cloud_core.md) · [Cloud — AWS](./16_cloud_aws.md) · [Cloud — GCP](./16_cloud_gcp.md) · [Cloud — Azure](./16_cloud_azure.md)

### Sections

- [Azure](#azure)

---

## Azure

<details>
  <summary>Azure</summary>

### Azure

**Definition:** Microsoft Azure — public cloud tightly integrated with Microsoft enterprise tools (Entra ID / Active Directory, Office 365, .NET).

**Why we need it:** Run backends in environments that already use Microsoft identity, contracts, and hybrid on-prem + cloud setups.

**When to use:**

| Service                           | Use case                                            |
| --------------------------------- | --------------------------------------------------- |
| **Virtual Machines**              | VMs (like EC2)                                      |
| **App Service**                   | PaaS for web APIs — deploy Node without managing OS |
| **Azure Functions**               | Serverless functions                                |
| **Blob Storage**                  | Object storage (like S3)                            |
| **Azure Database for PostgreSQL** | Managed Postgres                                    |
| **Azure Cache for Redis**         | Managed Redis                                       |
| **Service Bus**                   | Queues and topics for messaging                     |
| **AKS**                           | Managed Kubernetes                                  |
| **Azure CDN**                     | Content delivery                                    |
| **Microsoft Entra ID**            | SSO and app identity                                |

**Advantages:**

| Advantage                     | What you gain                                      |
| ----------------------------- | -------------------------------------------------- |
| **Enterprise AD integration** | Same login for corp users and internal tools       |
| **App Service**               | Simple Node deploy with slots (staging/production) |
| **Hybrid cloud**              | Connect on-prem datacenter to Azure (ExpressRoute) |
| **Enterprise agreements**     | Volume licensing for large orgs                    |

**Pluses and minuses:**

| +                                  | −                                               |
| ---------------------------------- | ----------------------------------------------- |
| Strong in .NET and enterprise      | Portal and naming can feel inconsistent         |
| Service Bus for reliable messaging | Node-specific examples fewer than AWS in places |
| Good compliance certifications     | Cold start and region choice need planning      |

**Examples (conceptual stack):**

```
Static Web Apps or Blob + CDN (frontend)
       ↓
App Service (Node API)
       ↓
Azure Database for PostgreSQL + Azure Cache for Redis
       ↓
Service Bus → Azure Function (email worker)
```

**Thoughts:** If the team is **TypeScript/Node** without Microsoft stack, AWS or GCP are equally valid — pick Azure when **identity and enterprise contract** already point there. Map Service Bus to queue patterns in [RabbitMQ](../12_message_broker_rabbitmq.md) / [message broker core](../12_message_broker_core.md).

**Sources:** [Azure documentation](https://learn.microsoft.com/azure/)

---

</details>
