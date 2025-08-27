<!-- Copyright (c) 2025 https://github.com/rasmabayu. All rights reserved. -->
# Architecture — RabbitMQ Advanced HA on Kubernetes

Components:
- RabbitMQ Cluster Operator: manages RabbitmqCluster CR
- RabbitMQ brokers: StatefulSet-like pods managed by operator, each with PVC
- Storage: PVC per broker (use fast SSD-backed StorageClass)
- Prometheus (ServiceMonitor) scraping rabbitmq_prometheus metrics
- Consumers/Producers: separate deployments with appropriate affinity & horizontal scaling

Key patterns:
- Quorum queues for critical durable queues
- PodDisruptionBudget to limit voluntary disruptions
- PodAntiAffinity to spread pods across nodes
- Backup via PVC snapshots or logical export of definitions
