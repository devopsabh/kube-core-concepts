# Kubernetes Learning Journey 🚀

This repository contains my personal collection of Kubernetes manifests, organized by resource type as I learn to orchestrate containerized applications.

## 📂 Project Structure

### [Pod/](./Pod/)
**The Atomic Unit.** Pods are the smallest deployable units in Kubernetes.
*   **Purpose:** Runs one or more containers (like Nginx or Node.js) with shared storage and network.
*   **Key Files:** 
    *   `instructionpod.yml`: Basic pod structure.
    *   `nodejspod.yml`: A Node.js runtime example.
    *   `redispod.yml`: A standalone Redis database.

### [ReplicaSet/](./ReplicaSet/)
**The Babysitter.** Ensures a specific number of pod replicas are running at all times.
*   **Purpose:** Self-healing. If a pod crashes, the ReplicaSet immediately creates a new one to match the `replicas` count.
*   **Key Concept:** The `selector` (magnet) must match the pod `labels` for the manager to find its workers.

### [Deployment/](./Deployment/)
**The Manager.** The real workhorse used for production apps.
*   **Purpose:** Manages ReplicaSets and Pods declaratively. It handles **Rolling Updates** (updating your app with zero downtime) and **Rollbacks** if something goes wrong.
*   **Best Practice:** Always use Deployments instead of raw Pods or ReplicaSets for high availability.

## 🛠️ How to Use
To apply any of these manifests to your cluster, use:
```bash
kubectl apply -f <folder_name>/<file_name>.yml

**## To check the status: **
kubectl get pods
kubectl get deployments
