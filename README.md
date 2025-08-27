<!-- Copyright (c) 2025 https://github.com/rasmabayu. All rights reserved. -->
# RabbitMQ Cluster — High Availability on Kubernetes🚀

Reference deployment for a production-grade **RabbitMQ cluster** with advanced high-availability features on Kubernetes.

## 🎯 Goals
- 🟢 **Highly available** RabbitMQ cluster (operator-managed)  
- 💾 Persistent storage with StorageClass-aware PVCs  
- 🔁 Quorum queues / mirrored queues policy for durability  
- 📊 Metrics & observability (Prometheus ServiceMonitor + Grafana dashboard placeholder)  
- 💽 Backup & restore guidance (snapshot + PVC backup hint)  
- 📈 Autoscaling considerations for consumers (not brokers)  

## 📂 What is included
- `manifests/`:
  - 📦 `operator-install.yaml` — instructions to install RabbitMQ Cluster Operator (Helm commands in README)  
  - 🐇 `rabbitmq-cluster.yaml` — RabbitmqCluster CR with resource tuning, persistence, podDisruptionBudget  
  - 📡 `service-monitor.yaml` — Prometheus ServiceMonitor for metrics scraping  
  - ⚖️ `queue-policy.yaml` — example management policy to enforce quorum queues  
  - 💾 `backup-job.yaml` — example job to snapshot definitions (placeholder)  
- `examples/`:
  - ➕ `producer-deployment.yaml` — example producer  
  - ➖ `consumer-deployment.yaml` — example consumer (for load testing)  
- `docs/`:
  - 🏗️ `architecture.md` — architecture explanation  
  - 📖 `runbook.md` — operational notes  

## ⚡ Quick Deploy (example)

**Prereqs:**  
🔧 `kubectl`, `helm`, access to cluster with StorageClass (dynamic provisioning), `prometheus-operator` (kube-prometheus-stack) installed.

1. 🚀 Install RabbitMQ Cluster Operator (official):
   ```bash
   helm repo add rabbitmq https://charts.rabbitmq.com
   helm repo update
   helm install rabbitmq-operator rabbitmq/rabbitmq-operator --create-namespace -n rabbitmq-system
   ```

2. 🐇 Apply RabbitmqCluster CR:
   ```bash
   kubectl apply -f manifests/rabbitmq-cluster.yaml -n messaging
   ```

3. 📡 Apply ServiceMonitor (if using Prometheus Operator):
   ```bash
   kubectl apply -f manifests/service-monitor.yaml -n monitoring
   ```

4. ⚖️ Create queue policy (requires rabbitmqctl or HTTP API):
   ```bash
   kubectl apply -f manifests/queue-policy-job.yaml -n messaging
   ```

5. ➕➖ Deploy example producer/consumer to test throughput (see examples/).  

## 📝 Notes & recommendations
- Use **quorum queues** for critical queues (durable, replicated at raft-level).  
- Avoid single large queues; partition workload across multiple queues and consumers.  
- Use PodDisruptionBudget and proper PodAntiAffinity to spread pods across nodes.  
- For DR, replicate at application layer or use federation between clusters in different regions.  
