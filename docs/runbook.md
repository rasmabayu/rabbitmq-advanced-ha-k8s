# Runbook (operations)

- Scaling brokers: increase replicas (note: adding brokers requires rebalance and consideration)
- Increasing durability: prefer quorum queues; avoid mirrored classic queues for new workloads
- Upgrades: use operator-managed rolling upgrades; monitor under-replicated queues
- Recovery: if a node fails, PVCs reattach to replacement pods; ensure StorageClass supports reclaimPolicy: Retain if you want manual recovery
