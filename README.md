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

### 🚀 [Advanced Deployment Concepts](./Advanced-Concepts/)
**The Strategy.** Managing how your application evolves and recovers in production.

*   **Rollouts & Updates:** Deployments allow you to update your app with zero downtime.
    *   **Restart:** `kubectl rollout restart deployment/<name>`
    *   **Update Image:** `kubectl set image deployment/<name> <container>=<new_image>`
    *   **Check Status:** `kubectl rollout status deployment/<name>`
    *   **Rollback:** `kubectl rollout undo deployment/<name>`
*   **Self-Healing:** The "Why" behind Kubernetes. If you set `replicas: 3`, the **ReplicaSet** (the babysitter) ensures the *Actual State* always matches your *Desired State*. If a Pod dies, a new one is born instantly.
*   **Understanding READY Status:**
    *   **In Pods (1/1):** Indicates 1 out of 1 container is healthy inside that specific Pod.
    *   **In Deployments (3/3):** Indicates 3 out of 3 total replicas are healthy across the cluster.
*   **Common Gotchas ⚠️:**
    *   **Label Mismatch:** If `labels` and `matchLabels` don't match, the Deployment can't "find" its Pods.
    *   **Immediate Exit:** Lightweight images like `busybox` will crash/exit immediately unless given a long-running task (e.g., `command: ["sleep", "3600"]`).
