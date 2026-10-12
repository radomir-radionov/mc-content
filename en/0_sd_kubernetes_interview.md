# Kubernetes — Interview

Answers are filled.

[← Back to hub](./0_system_design.md)

### Related docs

- [Kubernetes — Interview](./0_sd_kubernetes_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Kubernetes](https://devinterview.io/questions/software-architecture-and-system-design/kubernetes-interview-questions/).

### Kubernetes Basics

<details>
<summary>What is Kubernetes, and why is it used for container orchestration?</summary>

### What is Kubernetes, and why is it used for container orchestration?

**Definition:**

Kubernetes is a control plane that runs containers across a cluster and keeps them at the state you declared. Callers use a stable name while Pods are rescheduled, restarted, or replaced.

**How it works:**

- A Pod is the thing that gets scheduled: one or more containers that share a network namespace and volumes. Deployments and ReplicaSets keep a chosen number of those Pods running.
- Each Pod has its own IP. Services, Ingress, and NetworkPolicy sit above that so clients do not track Pod addresses.
- PersistentVolumes, claims, and StorageClasses attach disks that outlive a Pod, including disks created on demand.
- The Horizontal Pod Autoscaler changes the replica count from CPU or memory. A cluster autoscaler adds or removes nodes.
- ResourceQuota and LimitRange cap CPU and memory for a namespace or a container.
- Jobs and CronJobs run a batch to completion instead of staying up as a server.
- Rolling updates swap Pods in batches. A PodDisruptionBudget limits how many may be down during maintenance.
- A failed liveness probe restarts a container. A failed readiness probe keeps traffic off that Pod.
- Secrets and ConfigMaps inject credentials and settings without baking them into the image.
- Labels select groups of objects. Annotations carry extra metadata that selectors ignore.
- Custom controllers can watch the API and reconcile kinds you define yourself.
- The same API runs on a laptop, on premises, or in a cloud. A tool such as KubeFed can push shared configuration to more than one cluster.
- Packing work onto nodes cuts spare capacity, and the same manifests deploy in every environment.

**Examples:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: checkout
spec:
  containers:
    - name: api
      image: checkout:1.4
    - name: logs
      image: log-shipper:2
```

The API container and the log shipper are placed together and share localhost.
</details>

<details>
<summary>Describe the roles of master and worker nodes in the Kubernetes architecture.</summary>

### Describe the roles of master and worker nodes in the Kubernetes architecture.

**Definition:**

The cluster splits into a control plane that decides what should run, and worker nodes that run the containers.

**How it works:**

- The control plane, still often called the master, runs the API server, the scheduler, and the controller manager. etcd stores cluster state and is often its own highly available set.
- The API server is the only front door. kubectl and every controller talk to it.
- The scheduler picks a worker with enough CPU, memory, and matching constraints.
- Controllers watch live objects and create or delete Pods until the cluster matches the spec.
- Several control-plane machines can run those processes. User containers usually stay off them.
- Each worker runs kubelet, which starts the containers for assigned Pods, and kube-proxy, which programs Service forwarding.
- The container runtime on the worker (containerd or CRI-O) is what creates the processes.
- Some clusters add machines only for storage or ingress. Those are optional, not a third required role.

**Examples:**

`kubectl get nodes` marks control-plane machines with a control-plane taint and leaves the rest as workers. A new Pod is stored by the API server, placed by the scheduler, and started by kubelet on the chosen worker.
</details>

<details>
<summary>How do Pods facilitate the management of containers in Kubernetes?</summary>

### How do Pods facilitate the management of containers in Kubernetes?

**Definition:**

A Pod is the unit the scheduler places on a node. It wraps one or more containers that must run together and share a network and volumes.

**How it works:**

- Containers in the Pod share one IP, one hostname, and one network namespace, so they reach each other on localhost.
- The Pod is scheduled as a whole: all of its containers land on the same node, or none do.
- Volumes declared on the Pod can be mounted into every container in it.
- Each container still has its own image, command, and CPU or memory request.
- A single-container Pod is the usual case for a process that stands alone.
- Extra containers are sidecars: a proxy, a log collector, or a file sync next to the main process.
- kubelet can restart a container under the Pod’s restart policy. The Pod identity stays until the Pod itself is replaced.
- Deployments, Jobs, and DaemonSets do not schedule containers directly. They stamp out Pods from a template.

**Examples:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web
spec:
  containers:
    - name: nginx
      image: nginx:1.27
      ports:
        - containerPort: 80
    - name: metrics
      image: exporter:1
```

The exporter calls nginx at `127.0.0.1:80` because both containers share the Pod network.
</details>

<details>
<summary>What types of Services exist in Kubernetes, and how do they facilitate pod communication?</summary>

### What types of Services exist in Kubernetes, and how do they facilitate pod communication?

**Definition:**

A Service gives a stable name and address for a changing set of Pods. The type decides who can use that address.

**How it works:**

- ClusterIP, the default, allocates a virtual IP inside the cluster and balances across ready Pods that match the selector.
- NodePort also opens that port on every node, so traffic can enter from outside and is forwarded to those Pods.
- LoadBalancer asks the cloud or a controller for an external balancer in front of the Service.
- ExternalName publishes a CNAME to a DNS name outside the cluster and does not select Pods.
- A headless Service (`clusterIP: None`) returns the Pod IPs themselves. StatefulSets use that for stable per-Pod DNS.
- The selector fills an EndpointSlice. Pods that fail readiness drop out, so clients keep one address while membership changes.

**Examples:**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: payments
spec:
  type: ClusterIP
  selector:
    app: payments
  ports:
    - port: 80
      targetPort: 8080
```

Callers use `http://payments` and never store a Pod IP.
</details>

<details>
<summary>Explain the functionality of Namespaces in Kubernetes.</summary>

### Explain the functionality of Namespaces in Kubernetes.

**Definition:**

A Namespace is a naming and policy boundary inside one cluster, so teams or environments can share the machines without name collisions.

**How it works:**

- Names of Pods, Services, and most other objects only need to be unique inside the Namespace. `api` in `dev` and `api` in `prod` are different objects.
- ResourceQuota caps total CPU, memory, and object counts. LimitRange supplies default requests when a container omits them.
- Role bindings can be limited to one Namespace, and NetworkPolicy can restrict which Namespaces may talk.
- DNS includes the Namespace (`api.dev.svc.cluster.local`). The short name `api` resolves only in the same Namespace.
- A Namespace does not split nodes or the physical network by itself. Quotas and policies are what make the boundary real.
- Nodes and PersistentVolumes are cluster-scoped and do not live in a Namespace.

**Examples:**

`kubectl create namespace staging` and then `kubectl -n staging apply -f app.yaml` keeps those objects out of `default`. A quota of 4 CPU in `staging` rejects the next Pod that would exceed it.
</details>

### Kubernetes Objects and Workloads

<details>
<summary>Differentiate between Deployments, StatefulSets, and DaemonSets.</summary>

### Differentiate between Deployments, StatefulSets, and DaemonSets.

**Definition:**

Deployments, StatefulSets, and DaemonSets all keep Pods running. They differ in identity, storage, and where the Pods are placed.

**How it works:**

- A Deployment owns a ReplicaSet of interchangeable Pods. Any replica can serve any request, storage is usually external, and a rollout replaces Pods in batches. Peers talk through a Service, not through stable Pod names.
- A StatefulSet gives each Pod a stable ordinal (`db-0`, `db-1`) and usually its own volume via `volumeClaimTemplates`. It creates and deletes them in order. Headless DNS keeps those names across restarts.
- A DaemonSet runs one Pod on every selected node, including nodes that join later. There is no replica count to scale, and the Pods rarely need to find each other. Node agents such as log collectors fit this.
- Pick a Deployment for stateless replicas, a StatefulSet when identity and disk must survive reschedule, and a DaemonSet when the work is per node.
- A one-shot task is a Job, not one of these three.

**Examples:**

Three web replicas are a Deployment. A database where `db-0` is primary is a StatefulSet. A log collector on every machine is a DaemonSet.
</details>

<details>
<summary>How do ReplicaSets ensure pod availability?</summary>

### How do ReplicaSets ensure pod availability?

**Definition:**

A ReplicaSet keeps the number of running Pods equal to a declared count by creating replacements from a Pod template.

**How it works:**

- The template is the Pod spec copied for every new replica, including labels and the container image.
- A controller compares Pods that match the selector with `spec.replicas`.
- If a Pod disappears or its node is lost, the controller creates another from the template.
- Raising or lowering `replicas` scales the set up or down.
- A Deployment creates a fresh ReplicaSet for each rollout and moves replicas from the old set to the new one, which is what makes rollback a scale operation.
- The selector must match the template labels. Otherwise the controller will not adopt the Pods it creates.

**Examples:**

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: cache
spec:
  replicas: 2
  selector:
    matchLabels:
      app: cache
  template:
    metadata:
      labels:
        app: cache
    spec:
      containers:
        - name: redis
          image: redis:7
```

Delete one Pod and a new `redis:7` Pod appears without a manual restart.
</details>

<details>
<summary>What are Jobs in Kubernetes, and when is it suitable to use them?</summary>

### What are Jobs in Kubernetes, and when is it suitable to use them?

**Definition:**

A Job runs Pods until a set number finish successfully, then stops. It fits batch work, not a process that should stay up.

**How it works:**

- `completions` is how many successful Pods you need. `parallelism` is how many may run at once.
- A Pod that exits 0 is not restarted. A failed Pod can be retried up to `backoffLimit`.
- Finished Pods remain until a history limit or a TTL controller removes them.
- `completions: 1` and `parallelism: 1` is a single run, such as a schema migration.
- Higher numbers run independent chunks in parallel, such as compiling packages or processing files.
- A work-queue Job lets workers pull tasks until the queue is drained, often with indexed completion so each index is a distinct item.
- A CronJob creates a Job on a schedule for backups and other recurring batches.

**Examples:**

A nightly export is a CronJob whose template sets `completions: 1`. An import of 20 shards uses `completions: 20` and `parallelism: 4`, so four workers run until twenty have succeeded.
</details>

<details>
<summary>How do Labels and Selectors work together in Kubernetes?</summary>

### How do Labels and Selectors work together in Kubernetes?

**Definition:**

Labels are key-value tags on objects. Selectors are queries that find objects by those tags, so a Service or controller does not hard-code Pod names.

**How it works:**

- Labels such as `app` or `env` identify a group. They are not free-form notes.
- `matchLabels` requires every listed pair. Set-based selectors add `In`, `NotIn`, and `Exists`.
- Services, ReplicaSets, Deployments, DaemonSets, Jobs, StatefulSets, and NetworkPolicies all select Pods this way.
- A Service selector fills EndpointSlices. Ingress routes to Services, not by Pod label directly.
- Changing a label moves an object into or out of a group without editing the consumer.
- Annotations hold extra data that selectors ignore.
- A Service with no selector does not build endpoints automatically. An empty selector is not “match everything.”

**Examples:**

```yaml
metadata:
  labels:
    app: shop
    env: prod
---
spec:
  selector:
    matchLabels:
      app: shop
      env: prod
```

A Deployment, a Service, and a NetworkPolicy can share that pair.
</details>

<details>
<summary>What would you consider when performing a rolling update in Kubernetes?</summary>

### What would you consider when performing a rolling update in Kubernetes?

**Definition:**

A rolling update replaces old Pods with new ones in small batches so some capacity stays up the whole time.

**How it works:**

- The Deployment creates a new ReplicaSet and scales it up while it scales the previous one down.
- `maxUnavailable` is how far the ready count may fall below the desired count. `maxSurge` is how many extra Pods may exist during the swap.
- A new Pod receives traffic only after its readiness probe succeeds.
- A failing liveness probe restarts that container. If the new ReplicaSet never becomes ready, the rollout pauses and you can scale back to the previous ReplicaSet.
- `kubectl rollout status` shows progress. A PodDisruptionBudget can also refuse voluntary eviction when too many Pods would be down.
- Both versions serve at once, so the new release has to tolerate the old one still running.

**Examples:**

```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxUnavailable: 0
    maxSurge: 1
```

With three replicas, Kubernetes starts one new Pod, waits until it is ready, then removes one old Pod.
</details>

### Kubernetes Networking

<details>
<summary>How do Services route traffic to pods in Kubernetes?</summary>

### How do Services route traffic to pods in Kubernetes?

**Definition:**

A Service does not forward packets to a label. It keeps a list of ready Pod addresses, and the node redirects the Service IP to one of those addresses.

**How it works:**

- The selector matches Pod labels. The control plane writes the ready Pod IPs and ports into an EndpointSlice.
- kube-proxy, or another data plane, installs iptables, IPVS, or nftables rules so the ClusterIP is translated to a backend.
- Pods that fail readiness leave the list, so traffic stops before the process is deleted.
- `sessionAffinity: ClientIP` sticks a client address to one Pod for a timeout. `None`, the default, may pick a different backend per connection. This is address affinity, not a cookie.
- ClusterIP, NodePort, and LoadBalancer all use that endpoint list. ExternalName only returns a DNS CNAME.
- `port` is what clients use on the Service. `targetPort` is the container port.

**Examples:**

```yaml
spec:
  selector:
    app: shop
  sessionAffinity: ClientIP
  ports:
    - port: 80
      targetPort: 8080
```

A connection to the ClusterIP on port 80 lands on port 8080 of one ready Pod labeled `app=shop`.
</details>

<details>
<summary>Describe the purpose of Ingress and its key components.</summary>

### Describe the purpose of Ingress and its key components.

**Definition:**

Ingress describes HTTP and HTTPS routing from outside the cluster to Services, matched by host and path. The object does nothing until a controller implements it.

**How it works:**

- The Ingress resource lists hosts, paths, backend Services, and optional TLS Secrets.
- An Ingress controller (NGINX, Traefik, or a cloud controller) watches those objects and programs a layer-7 proxy or load balancer.
- Many Services can share one external address and one certificate.
- A rule can send `shop.example` `/api` to one Service and `/` to another.
- It fits clusters that expose several HTTP services through one entry. Non-HTTP traffic still uses a LoadBalancer or NodePort Service.
- The Gateway API is a newer model for the same job, with clearer roles and room for traffic splitting.

**Examples:**

One host, two paths: `/` goes to `web:80` and `/pay` goes to `payments:80`, with TLS from a Secret named `shop-tls`. Clients use `https://shop.example` and never see the ClusterIPs.
</details>

<details>
<summary>Explain Kubernetes DNS resolution for services.</summary>

### Explain Kubernetes DNS resolution for services.

**Definition:**

The cluster runs a DNS server, usually CoreDNS, that maps Service and Pod names to addresses so workloads call each other by name.

**How it works:**

- Early clusters used kube-dns. CoreDNS replaced it and turns features on as plugins.
- A Service `api` in Namespace `shop` is `api.shop.svc.cluster.local`. Inside that Namespace the short name `api` is enough.
- Pods with `dnsPolicy: ClusterFirst` send cluster names to CoreDNS and other names upstream. `dnsConfig` adds search domains or extra servers.
- An ordinary Service resolves to its ClusterIP. A headless Service resolves to the Pod IPs.
- CoreDNS watches the API, so the record follows address changes.
- The same short name in another Namespace is a different Service. Cross-namespace callers use the qualified name.
- Names across clusters are not built in. You add external DNS, Ingress, or a mesh.

**Examples:**

From a Pod in `shop`, `nslookup api` returns the ClusterIP. `nslookup api.billing.svc.cluster.local` reaches billing. A StatefulSet headless name `db-0.db.shop.svc.cluster.local` returns that Pod’s IP.
</details>

<details>
<summary>What is the Container Network Interface (CNI), and how does it integrate with Kubernetes?</summary>

### What is the Container Network Interface (CNI), and how does it integrate with Kubernetes?

**Definition:**

CNI is the plugin interface kubelet uses to attach a Pod to the network. Kubernetes does not ship the Pod network itself. The installed plugin does.

**How it works:**

- When the runtime creates a Pod sandbox, kubelet invokes the CNI binary configured under `/etc/cni/net.d`.
- The plugin assigns an IP, connects a veth or equivalent, and installs routes. On delete it removes them.
- Calico, Cilium, and Flannel are common plugins. They differ in overlay versus routed fabrics and in whether they enforce NetworkPolicy.
- The plugin is usually a DaemonSet, so every node runs it.
- Containers inside one Pod already share a namespace. CNI connects that namespace to the rest of the cluster.
- Multus is an optional meta-plugin that can add extra interfaces through NetworkAttachmentDefinitions. The default path is still one interface.
- Services and Ingress sit above the Pod network. CNI provides reachability. NetworkPolicy decides which flows are allowed.

**Examples:**

After a Pod is Running, `kubectl get pod -o wide` shows the IP the plugin assigned. If the CNI DaemonSet is down, new Pods stay in ContainerCreating with a network plugin error.
</details>

<details>
<summary>How do you implement Network Policies, and what are their benefits?</summary>

### How do you implement Network Policies, and what are their benefits?

**Definition:**

A NetworkPolicy selects Pods and lists which peers, ports, and protocols may connect. It limits lateral movement once a controller actually enforces it.

**How it works:**

- `podSelector` chooses the Pods. Ingress rules list who may connect in. Egress rules list where those Pods may connect out.
- A peer can be other Pods by label, another Namespace, or an IP block. Ports and protocols narrow the match.
- In the core API, once a policy selects a Pod for a direction, that direction becomes an allow list. Traffic no policy allows is dropped. Pods with no policy stay open.
- The objects do nothing until a plugin such as Calico or Cilium enforces them.
- Those plugins also add richer rules, including explicit deny. Cilium can match on application identity, not only IPs and ports.
- Add rules in small steps and test them. An ingress policy with no allow rule locks the Pod out.
- The policies document expected flows and give a review a concrete allow list.

**Examples:**

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: api-in
spec:
  podSelector:
    matchLabels:
      app: api
  policyTypes: [Ingress]
  ingress:
    - from:
        - podSelector:
            matchLabels:
              app: web
      ports:
        - protocol: TCP
          port: 8080
```

Only Pods labeled `app=web` may open TCP 8080 to `app=api`. Other ingress is denied.
</details>

### Kubernetes Storage

<details>
<summary>How do PersistentVolumes (PVs) and PersistentVolumeClaims (PVCs) interact in Kubernetes?</summary>

### How do PersistentVolumes (PVs) and PersistentVolumeClaims (PVCs) interact in Kubernetes?

**Definition:**

A PersistentVolume is cluster storage with a lifecycle of its own. A PersistentVolumeClaim is a user's request for size and access modes. The control plane binds one claim to one volume.

**How it works:**

- An administrator can create PVs statically, or a provisioner can create one for a claim. A PV is a cluster resource, like a node, and it outlives any single Pod.
- A claim asks for capacity and an access mode such as ReadWriteOnce, ReadOnlyMany, ReadWriteMany, or ReadWriteOncePod. A control loop binds it to a matching PV. The binding is one-to-one through a ClaimRef.
- A Pod does not name the PV. It mounts the claim. The cluster finds the bound volume and mounts that.
- If nothing matches, the claim stays unbound until a suitable volume exists. A 50Gi PV does not satisfy a 100Gi claim.
- Storage Object in Use Protection keeps a PVC that a Pod still uses, and a PV still bound to a PVC, from disappearing immediately.

**When to use:**

Use a claim whenever disk state must survive a Pod restart. Use an emptyDir volume when the data can vanish with the Pod.

**Examples:**

```text
PVC requests 30Gi ReadWriteOnce
controller binds a matching PV
Pod volumes.persistentVolumeClaim.claimName = that PVC
```

**Sources:**

- https://kubernetes.io/docs/concepts/storage/persistent-volumes/
</details>

<details>
<summary>Explain dynamic provisioning in relation to StorageClasses.</summary>

### Explain dynamic provisioning in relation to StorageClasses.

**Definition:**

Dynamic provisioning creates a PersistentVolume when a claim asks for a StorageClass and no static volume matches. The class is the administrator's offer of a storage type.

**How it works:**

- A StorageClass names a provisioner and parameters such as performance. The claim sets storageClassName to that class.
- If the claim asks for the empty class name, dynamic provisioning is off for that claim.
- Binding can be Immediate, the default, or WaitForFirstConsumer, which waits until a Pod that uses the claim exists so topology constraints are known.
- A dynamically provisioned volume takes the reclaim policy of its StorageClass. The default is Delete, so deleting the claim deletes the volume. Retain keeps the volume in the Released phase so an administrator can recover the data.
- The DefaultStorageClass admission plugin can assign a default class when the claim omits one.

**When to use:**

Use dynamic provisioning when users should request size and a class without asking an administrator to pre-create disks. Use static PVs when the disk already exists outside the cluster.

**Examples:**

```yaml
storageClassName: fast
accessModes: [ReadWriteOnce]
resources:
  requests:
    storage: 30Gi
```

**Sources:**

- https://kubernetes.io/docs/concepts/storage/dynamic-provisioning/
- https://kubernetes.io/docs/concepts/storage/storage-classes/
- https://kubernetes.io/docs/concepts/storage/persistent-volumes/
</details>

<details>
<summary>What strategies exist for handling persistent data when a pod’s lifecycle ends?</summary>

### What strategies exist for handling persistent data when a pod’s lifecycle ends?

**Definition:**

Pod deletion does not delete a bound PersistentVolume. What happens to the bytes depends on the claim, the reclaim policy, and whether a controller such as a StatefulSet still owns the claim.

**How it works:**

- A volume mounted from a claim stays after the Pod is gone. The next Pod that mounts the same claim sees the same data.
- Deleting the PVC starts reclaim. Retain leaves the PV Released and keeps the external disk. Delete removes the PV and, for plugins that support it, the external asset. Recycle is deprecated; dynamic provisioning is the recommended replacement.
- Storage protection delays removal while a Pod still uses the claim or the PV is still bound.
- Scaling a StatefulSet down deletes Pods and leaves their PVCs bound, so the data is still there when the ordinal comes back.

**When to use:**

Retain when the data is precious and a person must decide. Delete when the claim is the lifetime of the data. A StatefulSet claim per ordinal when identity and disk must stick together.

**Sources:**

- https://kubernetes.io/docs/concepts/storage/persistent-volumes/
- https://kubernetes.io/docs/tutorials/stateful-application/basic-stateful-set/
</details>

### Kubernetes Cluster Management

<details>
<summary>Describe the process for initializing a Kubernetes cluster.</summary>

### Describe the process for initializing a Kubernetes cluster.

**Definition:**

A cluster is a control plane plus worker nodes. Initialization stands up the API, the datastore, the schedulers and controllers, then nodes that can run Pods.

**How it works:**

- The control plane is kube-apiserver (the HTTP API), etcd (the consistent store for API data), kube-scheduler (binds unscheduled Pods to nodes), and kube-controller-manager (runs the controllers). A cloud-controller-manager is optional.
- Each node runs a kubelet, which keeps Pods and their containers running, a container runtime, and optionally kube-proxy for Service rules.
- Addons such as cluster DNS are not part of that core, but a usable cluster usually installs them after the control plane answers.
- The docs show minikube or a hosted playground when you only need a cluster to try an API. Production layout is a choice of how those same components are deployed.

**When to use:**

A single-node local cluster is enough to learn the API. More than one control-plane machine, with etcd kept highly available, is the shape for a cluster other people depend on.

**Sources:**

- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definitions/
</details>

<details>
<summary>What factors need to be considered when adding new nodes to a Kubernetes cluster?</summary>

### What factors need to be considered when adding new nodes to a Kubernetes cluster?

**Definition:**

A new node joins the cluster only after it runs the node components and the scheduler can place Pods on it. Capacity, labels, and taints decide what actually lands there.

**How it works:**

- The node needs a kubelet and a container runtime. kube-proxy is optional and maintains Service network rules.
- The scheduler places Pods that fit free CPU and memory. A node that is short on either will not receive Pods that request more.
- Labels let you target the node later with nodeSelector or affinity. Prefer label keys the kubelet cannot rewrite, such as the node-restriction.kubernetes.io/ prefix, when the label is a security or compliance boundary.
- A drained node carries the unschedulable taint. New Pods that do not tolerate it stay off that node. DaemonSets are the usual exception.
- Adding nodes does not by itself copy control-plane data. etcd and the API server remain the source of cluster state.

**Sources:**

- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/
- https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
</details>

<details>
<summary>How does draining a node affect the running applications, and when should you perform this action?</summary>

### How does draining a node affect the running applications, and when should you perform this action?

**Definition:**

Draining evicts Pods from a node so you can maintain the machine. Workloads that have a PodDisruptionBudget stay available only up to the budget you set.

**How it works:**

- kubectl drain evicts Pods, honors their termination grace period, and respects PodDisruptionBudgets. When it returns successfully, the evicted Pods are gone and the node is unschedulable.
- DaemonSet Pods are replaced by their controller, which also ignores the unschedulable taint. Pass --ignore-daemonsets or the drain will not finish those Pods for you.
- A Pod whose spec sets nodeName is bound to that node and can keep running there even after the drain.
- Do this before kernel, hardware, or VM maintenance. After maintenance, kubectl uncordon allows scheduling again.
- Drain one node per command. Parallel drains of different nodes still honor the same budget: a StatefulSet with minAvailable 2 will not drop below that.

**When to use:**

Drain before you reboot or delete the node. Do not drain when the remaining replicas cannot meet the disruption budget.

**Examples:**

```text
kubectl drain --ignore-daemonsets <node name>
# maintenance
kubectl uncordon <node name>
```

**Sources:**

- https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
</details>

<details>
<summary>In what scenarios would you scale a Kubernetes deployment, and what tools would you use?</summary>

### In what scenarios would you scale a Kubernetes deployment, and what tools would you use?

**Definition:**

Scale a Deployment when demand no longer matches the replica count or the CPU and memory each replica has. Kubernetes can change the count (horizontal) or the resources (vertical), by hand or with a controller.

**How it works:**

- Manual horizontal scale changes the replica count. Manual vertical scale patches the container resource requests.
- A HorizontalPodAutoscaler is a built-in resource and controller. It periodically sets the replica count from observed utilization such as CPU or memory.
- A VerticalPodAutoscaler is an add-on, not part of core Kubernetes. It needs the Metrics Server. As of the autoscaling docs, it does not yet resize Pods in place; in-place container resize is a separate stable feature you can apply manually.
- KEDA, a CNCF project, scales on events such as queue depth, or on a cron schedule. Cluster proportional autoscalers scale from the number of nodes or cores and are also separate projects.
- If more replicas still do not fit, scale the cluster by adding or removing nodes.

**When to use:**

HPA when the load is CPU, memory, or a metric that tracks request rate. KEDA when the signal is a queue or a schedule. More nodes when the cluster itself is full.

**Sources:**

- https://kubernetes.io/docs/concepts/workloads/autoscaling/
</details>

### Kubernetes Security

<details>
<summary>Explain Role-Based Access Control (RBAC) in Kubernetes.</summary>

### Explain Role-Based Access Control (RBAC) in Kubernetes.

**Definition:**

Kubernetes RBAC grants only the API verbs a user or ServiceAccount needs. Roles and bindings are the objects that express that grant.

**How it works:**

- Prefer a Role and a RoleBinding in one namespace. A ClusterRoleBinding hands the same rights in every namespace.
- Avoid wildcards, especially on all resources. Kubernetes is extensible, so a wildcard also covers resource types that do not exist yet.
- Do not use cluster-admin for daily work. Do not add people to system:masters: that group skips RBAC checks entirely.
- list and watch on Secrets reveal their contents, not only get. Permission to create Pods also lets someone mount Secrets and ConfigMaps in that namespace and run as any ServiceAccount there.
- Creating PersistentVolumes can mean a hostPath volume and thus the node filesystem. Leave PV creation to trusted operators and to the control plane's provisioners. Users take storage through claims.
- The escalate, bind, and impersonate verbs, plus CSR approval and serviceaccounts/token, can raise privilege. Review bindings on a schedule.

**When to use:**

Namespace RoleBindings for application teams. Cluster-wide rights only for the few controllers that must see every namespace.

**Sources:**

- https://kubernetes.io/docs/concepts/security/rbac-good-practices/
</details>

<details>
<summary>Describe how to securely manage Secrets in Kubernetes.</summary>

### Describe how to securely manage Secrets in Kubernetes.

**Definition:**

A Secret holds a small sensitive value, such as a password, token, or key, outside the image and the Pod spec. By default that object is not encrypted in etcd.

**How it works:**

- Secrets are stored unencrypted in etcd unless you enable encryption at rest. Anyone who can read the API or etcd can read them. Anyone who can create a Pod in the namespace can read Secrets there, including by creating a Deployment.
- Enable encryption at rest, restrict RBAC to least privilege, limit which containers mount the Secret, and consider an external secret store.
- A Pod can take a Secret as an environment variable, a volume (including a dotfile key), or image-pull credentials so the kubelet can pull from a private registry.
- Do not put the same bytes in a ConfigMap. ConfigMaps are for non-confidential data and are not a secrecy mechanism.
- list and watch on Secrets return the data, so those verbs are as sensitive as get.

**Sources:**

- https://kubernetes.io/docs/concepts/configuration/secret/
- https://kubernetes.io/docs/concepts/security/rbac-good-practices/
- https://kubernetes.io/docs/concepts/configuration/configmap/
</details>

<details>
<summary>What is a security context, and how does it apply to pod security?</summary>

### What is a security context, and how does it apply to pod security?

**Definition:**

Pod security is the set of restrictions that stop a workload from turning into node access. Kubernetes enforces that with admission policy and by not granting powerful ServiceAccounts, not only with a field on one container.

**How it works:**

- Permission to create Pods is already powerful: the Pod can mount Secrets and ConfigMaps and can run as any ServiceAccount in the namespace. A privileged Pod can reach the node.
- Where you do not fully trust the caller, enforce the Baseline or Restricted Pod Security Standard, with Pod Security admission or an equivalent control.
- Do not run powerful Pods next to untrusted ones. Taints, tolerations, node affinity, or pod anti-affinity keep them apart. DaemonSets that are privileged should be few and least-privilege.
- Set automountServiceAccountToken to false when the Pod does not need to call the API.
- Namespace boundaries are the real tenancy line. Rights inside one namespace are a weak wall.

**When to use:**

Restricted for workloads that face the network. Baseline when the image still needs a few host capabilities you have reviewed. Privileged only for the control components that must touch the node.

**Sources:**

- https://kubernetes.io/docs/concepts/security/rbac-good-practices/
</details>

<details>
<summary>How do NetworkPolicies contribute to pod security, and how would you implement them?</summary>

### How do NetworkPolicies contribute to pod security, and how would you implement them?

**Definition:**

A NetworkPolicy limits which connections a Pod may accept or open, at IP and port, for TCP, UDP, and SCTP. It does nothing unless the cluster network plugin implements it.

**How it works:**

- By default a Pod is not isolated: all ingress and all egress are allowed. A policy that selects the Pod and lists Ingress or Egress in policyTypes isolates that direction.
- Once isolated, only connections allowed by the union of applicable policies are permitted, plus reply traffic, plus traffic to and from the node. A Pod cannot block itself.
- Peers are other Pods, namespaces, or CIDR blocks, chosen with selectors. Both the source egress rule and the destination ingress rule must allow the connection.
- Policies add. Order does not matter.
- Put a default deny on a namespace, then allow the specific ports the app needs. A policy object with no enforcing plugin is a no-op.

**When to use:**

Use NetworkPolicy for east-west limits inside the cluster. It is not a substitute for RBAC, and it does not see traffic that never touches a Pod.

**Sources:**

- https://kubernetes.io/docs/concepts/services-networking/network-policies/
</details>

### Observability and Monitoring

<details>
<summary>Describe the role and capabilities of the Metrics Server in a Kubernetes cluster.</summary>

### Describe the role and capabilities of the Metrics Server in a Kubernetes cluster.

**Definition:**

The Metrics Server is the cluster addon that serves current CPU and memory numbers so autoscaling controllers can see utilization. It is not part of the core control plane.

**How it works:**

- HorizontalPodAutoscaler adjusts replicas from observed resource utilization such as CPU or memory. Those numbers have to come from a metrics pipeline.
- VerticalPodAutoscaler, which is itself an addon, requires the Metrics Server to be installed.
- The components overview lists container resource monitoring as an addon: something that collects and stores container metrics, separate from etcd and the API server.
- It answers "what is this Pod using now". It is not a long-term monitoring store.

**When to use:**

Install it when HPA or VPA must scale on CPU or memory. A separate monitoring system is what you keep for history and alerts.

**Sources:**

- https://kubernetes.io/docs/concepts/workloads/autoscaling/
- https://kubernetes.io/docs/concepts/overview/components/
</details>

<details>
<summary>How can Prometheus be configured to monitor Kubernetes components?</summary>

### How can Prometheus be configured to monitor Kubernetes components?

**Definition:**

Prometheus is not a Kubernetes component. The cluster expects an addon to collect container metrics; Prometheus is one system teams run beside the API server for that job.

**How it works:**

- Core components are the API server, etcd, scheduler, controller manager, kubelet, and the runtime. Their health is the control plane. Metrics collection is listed separately as an addon.
- Point the scraper at the components you already run, and keep it from needing cluster-admin. RBAC good practice is least privilege and no wildcard reads.
- Autoscaling still wants a short resource-metrics path, such as the Metrics Server, for CPU and memory. A monitoring addon is the place for history.
- Do not store dashboards in the same etcd as a substitute for the metrics addon. etcd holds API objects.

**When to use:**

Use it when you need a time series of cluster and workload metrics. Use the Metrics Server when the only consumer is HPA or VPA.

**Sources:**

- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/concepts/workloads/autoscaling/
- https://kubernetes.io/docs/concepts/security/rbac-good-practices/
</details>

<details>
<summary>Discuss the integration of Grafana for Kubernetes observability.</summary>

### Discuss the integration of Grafana for Kubernetes observability.

**Definition:**

Grafana is not part of Kubernetes. Observability in the cluster is an addon concern: resource metrics and cluster-level logs, read by something outside the control plane.

**How it works:**

- The components page names two relevant addons: container resource monitoring, and cluster-level logging that saves container logs to a central store.
- A dashboard reads those stores. It does not replace the API server, and it should not hold cluster-admin rights just to draw charts.
- HPA still scales from the resource-metrics pipeline, not from a dashboard query.
- Treat the dashboard's credentials like any other Secret: encryption at rest, least-privilege RBAC, and no copy in a ConfigMap.

**When to use:**

Use a dashboard when people need to look at the same series an addon already stores. Skip it when the only decision is an HPA replica count.

**Sources:**

- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/concepts/workloads/autoscaling/
- https://kubernetes.io/docs/concepts/configuration/secret/
</details>

### Kubernetes Scheduling and Scalability

<details>
<summary>How can you use Node Selectors and Affinity rules to influence pod placement?</summary>

### How can you use Node Selectors and Affinity rules to influence pod placement?

**Definition:**

nodeSelector and affinity tell the scheduler which nodes may run a Pod. The scheduler already spreads Pods onto nodes with free resources; these fields add a constraint.

**How it works:**

- nodeSelector is the simple form. The Pod runs only on nodes that have every listed label.
- Affinity is more expressive. A required rule must match. A preferred rule still allows scheduling when it cannot. Pod affinity and anti-affinity look at labels of other Pods, so two services can sit together or apart.
- nodeName skips the scheduler and binds the Pod to that node. A drained node still runs a Pod that sets nodeName.
- For isolation labels, use a prefix the kubelet cannot change. The NodeRestriction admission plugin blocks kubelet writes to node-restriction.kubernetes.io/.
- The scheduler is the control-plane component that performs this assignment.

**When to use:**

nodeSelector for a hard label such as a disk type. Preferred affinity when the placement is a bias. Anti-affinity when two copies must not share a failure domain.

**Sources:**

- https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/
- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
</details>

<details>
<summary>Explain Horizontal Pod Autoscaling (HPA) and its triggers within Kubernetes.</summary>

### Explain Horizontal Pod Autoscaling (HPA) and its triggers within Kubernetes.

**Definition:**

A HorizontalPodAutoscaler is a Kubernetes API object and controller that changes the replica count of a workload so it tracks observed utilization.

**How it works:**

- It periodically adjusts replicas of an object that manages Pods, such as a Deployment, from signals such as CPU or memory usage.
- That is horizontal scaling. Vertical scaling changes the resources of the existing Pods and is a different mechanism. VPA is an addon and needs the Metrics Server.
- Manual scaling still exists: change the replica count or patch resource requests without a controller.
- KEDA can scale the same idea from events, for example messages in a queue, or from a cron schedule. Those are not the built-in HPA triggers.
- If the extra Pods do not fit, the next step is more nodes, not a larger HPA number that cannot schedule.

**When to use:**

Use HPA when replica count should follow CPU, memory, or another utilization metric. Use a queue-based scaler when the trigger is backlog, not CPU.

**Sources:**

- https://kubernetes.io/docs/concepts/workloads/autoscaling/
</details>

### Kubernetes Troubleshooting and Debugging

<details>
<summary>What steps would you take to troubleshoot a pod that is not starting?</summary>

### What steps would you take to troubleshoot a pod that is not starting?

**Definition:**

A Pod starts when the scheduler can place it and the kubelet can run its containers. A Pod that never becomes Ready is stuck on one of those two steps.

**How it works:**

- If it is Pending, the scheduler has not bound it. Check requests against free node resources, nodeSelector and affinity, taints, and a PVC that is still unbound. A claim with WaitForFirstConsumer stays pending until placement is possible. A claim that asks for more than any PV will wait forever.
- If it is assigned but containers do not run, the kubelet or the runtime failed: the image cannot be pulled, or a referenced Secret or ConfigMap is missing. Image pull from a private registry needs a Secret the kubelet can use.
- A NetworkPolicy does not stop a container from starting. It only drops connections after the Pod is up, and only if a plugin enforces it.
- kubectl drain is the wrong tool here. Drain evicts Pods that are already running so you can maintain a node.
- Storage protection can leave a PVC in Terminating with the finalizer kubernetes.io/pvc-protection while a Pod still uses it. That blocks a replacement claim from binding the same name.

**Sources:**

- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/
- https://kubernetes.io/docs/concepts/storage/persistent-volumes/
- https://kubernetes.io/docs/concepts/configuration/secret/
- https://kubernetes.io/docs/concepts/services-networking/network-policies/
</details>

<details>
<summary>How would you diagnose network issues in Kubernetes?</summary>

### How would you diagnose network issues in Kubernetes?

**Definition:**

Kubernetes networking is kube-proxy Service rules plus whatever the cluster network plugin does. A NetworkPolicy only affects traffic when that plugin implements it.

**How it works:**

- kube-proxy on the node maintains the rules that implement Services. If it is not running, Service VIPs are the first thing to suspect. Pod IP traffic is the plugin's job, not kube-proxy's.
- By default Pods accept and open every connection. After a policy isolates a direction, only the union of matching rules applies, and both ends must allow the flow.
- A policy object in a cluster whose plugin does not enforce NetworkPolicy changes nothing. That looks like a policy bug and is a missing controller.
- Traffic to and from the node where the Pod runs is always allowed, regardless of the Pod IP. A Pod cannot block itself.
- DNS is an addon, not a control-plane component. Name lookup failing while IP lookup works points at that addon, not at the API server.

**Sources:**

- https://kubernetes.io/docs/concepts/services-networking/network-policies/
- https://kubernetes.io/docs/concepts/overview/components/
</details>

### Advanced Kubernetes Features

<details>
<summary>Define Custom Resource Definitions (CRDs) and their utility in Kubernetes.</summary>

### Define Custom Resource Definitions (CRDs) and their utility in Kubernetes.

**Definition:**

A CustomResourceDefinition adds a resource type to the API server. After you create the CRD, the API grows a REST path for each version you list.

**How it works:**

- The CRD name is plural.group. spec.group and spec.versions define the REST path /apis/<group>/<version>. spec.scope is namespaced or cluster-scoped.
- Deleting a namespace deletes the custom objects in it. The CRD object itself is not namespaced.
- Each version has a served flag. The API server only exposes versions you mark served.
- Controllers and addons use CRDs for behavior that is not a built-in Pod or Service. The autoscaling docs note that VPA is installed as custom resources, not as core API types.
- CRDs are available from Kubernetes 1.16 in the current task page. Check kubectl version before you apply one written for a newer API.

**When to use:**

Add a CRD when a controller must store its own objects in the API. Use a ConfigMap when you only need key-value settings for a Pod.

**Sources:**

- https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definitions/
- https://kubernetes.io/docs/concepts/workloads/autoscaling/
</details>

<details>
<summary>Discuss the functionalities provided by a Service Mesh like Istio in Kubernetes.</summary>

### Discuss the functionalities provided by a Service Mesh like Istio in Kubernetes.

**Definition:**

A service mesh is not a Kubernetes control-plane component. Kubernetes already gives you Services, kube-proxy, and NetworkPolicy; a mesh is an extra layer teams add beside that.

**How it works:**

- kube-proxy implements Service forwarding on the node. NetworkPolicy, when the plugin enforces it, allows or denies connections by Pod, namespace, or CIDR.
- Those two mechanisms do not retry a call, shift traffic between versions, or add mutual identity on the connection. That gap is why a mesh is deployed as separate software.
- Give the mesh the same RBAC treatment as any powerful addon: no cluster-admin for daily traffic, and no wildcard read of Secrets.
- A mesh sidecar that mounts every Secret in the namespace is the same risk the RBAC page describes for any Pod creator.
- Keep the mesh's config in ConfigMaps for non-secret settings and Secrets for credentials.

**When to use:**

Add a mesh when you need behavior the Service and NetworkPolicy APIs do not provide. Stay with Services and NetworkPolicy when allow-lists and virtual IPs are enough.

**Sources:**

- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/concepts/services-networking/network-policies/
- https://kubernetes.io/docs/concepts/security/rbac-good-practices/
- https://kubernetes.io/docs/concepts/configuration/secret/
</details>

<details>
<summary>What are the use cases for Kubernetes Federation?</summary>

### What are the use cases for Kubernetes Federation?

**Definition:**

Federation is a way to point several clusters at a shared problem. Each cluster still has its own control plane: its own API server and its own etcd.

**How it works:**

- One cluster's scheduler only places Pods on that cluster's nodes. A second region is a second cluster, not a label on a node.
- You federate when an application must run in more than one of those clusters: a second failure domain, or users close to more than one region.
- Node affinity and topology spread only move Pods inside one cluster. They do not replace a second control plane.
- RBAC, Secrets, and NetworkPolicy do not cross clusters. Each cluster needs its own least-privilege bindings and its own encryption at rest for Secrets.
- Adding nodes scales one cluster. Federation is the step after one cluster's failure domain is not enough.

**When to use:**

Use more than one cluster when a single control plane's outage or a single region is unacceptable. Use one cluster with topology spread when the copies only need different nodes.

**Sources:**

- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/
- https://kubernetes.io/docs/concepts/security/rbac-good-practices/
</details>

### Kubernetes Ecosystem and Tools

<details>
<summary>How does Helm enhance application management within Kubernetes?</summary>

### How does Helm enhance application management within Kubernetes?

**Definition:**

Helm is not a Kubernetes API. Kubernetes applies objects such as Deployments, Services, ConfigMaps, and CRDs; Helm is a client that renders those objects from a package and submits them.

**How it works:**

- A chart is a set of manifests parameterized for one environment. The same chart can set a different ConfigMap or replica count per cluster.
- The API server still stores the resulting objects. etcd does not know the chart existed.
- CRDs that a chart installs follow the CRD rules: the definition is cluster-scoped, and namespaced custom objects die with the namespace.
- Secrets a chart creates are still unencrypted in etcd until you turn on encryption at rest, and they still need least-privilege RBAC.
- Prefer the chart when many objects must be installed together. Prefer a single manifest when the release is one Deployment.

**Sources:**

- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definitions/
- https://kubernetes.io/docs/concepts/configuration/secret/
- https://kubernetes.io/docs/concepts/configuration/configmap/
</details>

<details>
<summary>Explain the role of Kustomize in Kubernetes configuration management.</summary>

### Explain the role of Kustomize in Kubernetes configuration management.

**Definition:**

Kustomize patches plain Kubernetes manifests per environment without turning them into a template language. The API server still receives ordinary objects.

**How it works:**

- Base manifests hold the Deployment, Service, ConfigMap, or Secret. An overlay changes names, labels, or a few fields for one environment.
- ConfigMap data is non-confidential and capped at 1 MiB. Confidential values stay in Secrets, with encryption at rest.
- A ConfigMap mounted as a volume is eventually updated by the kubelet. The same data consumed as environment variables is not; the Pod must restart. A subPath mount also does not see updates.
- immutable: true on a ConfigMap or Secret blocks changes to data. You delete and recreate the object. That cuts apiserver watch load when thousands of Pods mount them.
- The Pod and the ConfigMap must share a namespace, unless the app reads the API itself.

**When to use:**

Use overlays when the environments differ by a small patch. Use a chart when you want a packaged install with a recorded release.

**Sources:**

- https://kubernetes.io/docs/concepts/configuration/configmap/
- https://kubernetes.io/docs/concepts/configuration/secret/
</details>

<details>
<summary>Describe the importance of a Container Registry in a Kubernetes environment.</summary>

### Describe the importance of a Container Registry in a Kubernetes environment.

**Definition:**

The container runtime on each node pulls images and runs them. A registry is where those images live; the kubelet pulls them, including from a private registry with a Secret.

**How it works:**

- Node components are the kubelet, an optional kube-proxy, and a container runtime. Without a runtime there is nothing to start the image.
- A Secret can hold registry credentials so the kubelet can pull a private image. That Secret is still sensitive: encryption at rest and least-privilege RBAC apply.
- Anyone who can create a Pod in the namespace can use Secrets there, so a private registry Secret is readable by that person.
- Image choice is separate from scheduling. nodeSelector can still require a node that has a local disk or a label, but it does not store the image.
- Do not bake passwords into the image. The Secret exists so confidential data stays out of the image and the Pod spec.

**Sources:**

- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/concepts/configuration/secret/
- https://kubernetes.io/docs/concepts/security/rbac-good-practices/
</details>

### Best Practices in Kubernetes

<details>
<summary>What best practices would you follow for high availability in Kubernetes deployments?</summary>

### What best practices would you follow for high availability in Kubernetes deployments?

**Definition:**

High availability means the control plane keeps answering and the workload keeps a quorum of ready Pods. etcd is the consistent store those decisions depend on.

**How it works:**

- Run the control plane so the API server and etcd stay available. The components page calls etcd a consistent and highly available key-value store for all API data. A single etcd is a single place where that data can stop.
- Spread replicas with anti-affinity or topology constraints so one node loss is not every replica. The scheduler already avoids nodes that lack free resources.
- Set a PodDisruptionBudget before anyone drains a node. Drain refuses to evict past minAvailable.
- HPA replaces replicas when load grows. It does not replace a node that disappeared; that still needs capacity elsewhere.
- Keep Secrets encrypted at rest and RBAC narrow, so a spare control-plane node is not also a spare copy of every password in clear text.

**Sources:**

- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
- https://kubernetes.io/docs/concepts/workloads/autoscaling/
- https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/
- https://kubernetes.io/docs/concepts/configuration/secret/
</details>

<details>
<summary>How do ConfigMaps and Secrets improve configuration management?</summary>

### How do ConfigMaps and Secrets improve configuration management?

**Definition:**

ConfigMaps hold non-confidential key-value settings. Secrets hold small confidential values. Both let you change configuration without rebuilding the image.

**How it works:**

- A ConfigMap has data for UTF-8 strings and binaryData for base64. Keys must not overlap. The object cannot exceed 1 MiB. It is not encrypted and is the wrong place for a password.
- A Pod in the same namespace can take a ConfigMap as container args, environment variables, or files on a read-only volume. Volume mounts eventually refresh; environment variables do not, and neither does a subPath mount.
- A Secret is the confidential counterpart: passwords, tokens, keys, and private registry credentials. Enable encryption at rest, or etcd stores them in the clear.
- immutable: true freezes data, which avoids accidental edits and reduces apiserver watches when many Pods mount the object.
- A static Pod spec cannot refer to a ConfigMap or other API objects.

**When to use:**

ConfigMap for hosts, feature flags, and files the whole namespace may read. Secret for anything that should not appear in a manifest review.

**Examples:**

```yaml
# ConfigMap
data:
  DATABASE_HOST: db.internal
# Secret — confidential, encrypt etcd at rest
```

**Sources:**

- https://kubernetes.io/docs/concepts/configuration/configmap/
- https://kubernetes.io/docs/concepts/configuration/secret/
</details>

<details>
<summary>Describe multitenancy considerations for resource utilization in Kubernetes.</summary>

### Describe multitenancy considerations for resource utilization in Kubernetes.

**Definition:**

Namespaces are the Kubernetes boundary between tenants. They are a weak wall for anything a Pod inside the namespace can mount, so utilization limits and admission policy have to sit on top.

**How it works:**

- Separate trust levels into different namespaces. Rights inside one namespace should be treated as shared: a Pod creator can read Secrets and ConfigMaps there and can run as any ServiceAccount in that namespace.
- Resource quotas limit how many objects a tenant can create. The RBAC page notes that unrestricted object creation can exhaust etcd; quotas are the mitigation it names.
- Do not let tenants create PersistentVolumes. A hostPath PV is access to the node disk. Tenants request storage with claims.
- NetworkPolicy is per namespace and additive, but only if the plugin enforces it. Without that, tenants share an open network.
- HPA and requests keep one tenant's load from scheduling onto every free core only if requests are set. A Pod with no requests is easy to place and easy to starve neighbors.
- Label namespaces carefully. The RBAC page warns that a user who can patch namespace labels may weaken Pod Security or NetworkPolicy selection.

**Sources:**

- https://kubernetes.io/docs/concepts/security/rbac-good-practices/
- https://kubernetes.io/docs/concepts/services-networking/network-policies/
- https://kubernetes.io/docs/concepts/workloads/autoscaling/
- https://kubernetes.io/docs/concepts/storage/persistent-volumes/
</details>
