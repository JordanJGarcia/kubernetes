# [Kubernetes Overview](https://kubernetes.io/docs/concepts/overview/)

Portable, extensible, open source platform for managing containerized workloads and services.

It is a framework to run distributed systems resiliently.

## What it can do

Provides you with:

* Service discovery and load balancing
* Storage orchestration
* Automated rollouts and rollbacks
* Automatic bin packing
* Self-healing
* Secret and configuration management
* Batch execution
* Horizontal scaling
* IPv4/IPv6 dual-stack
* Designed for extensibility

## Components

A Kubernetes cluster consists of a control plane and one or more worker nodes.

### Control Plane Components

| Component | Purpose |
| --------- | ------- |
| kube-apiserver | The core component server that exposes the Kubernetes HTTP API |
| etcd | Consistent and highly-available key value store for all API server data |
| kube-scheduler | Looks for Pods not yet bound to a node, and assigns each Pod to a suitable node |
| kube-controller-manager | Runs controllers to implement Kubernetes API behavior |
| cloud-controller-manager | **(Optional)** Integrates with underlying cloud provider(s) |

### Node Components

| Component | Purpose |
| --------- | ------- |
| kubelet | Ensures that Pods are running, including their containers |
| kube-proxy | **(Optional)** Maintains network rules on nodes to implement Services |
| Container runtime | Software responsible for running containers |
