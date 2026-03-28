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

# 🚀 Advanced Deployment Concepts

## Rollouts & Updates
Deployments allow you to update your app without downtime.

* **Trigger a restart:** `kubectl rollout restart deployment/<name>`
* **Update Image:** `kubectl set image deployment/<name> <container>=<new_image>`
* **Check Status:** `kubectl rollout status deployment/<name>`
* **Undo/Rollback:** `kubectl rollout undo deployment/<name>`

---

## Self-Healing (The "Why")
When you set `replicas: 3`, the **ReplicaSet** (the babysitter) constantly checks if the *Actual State* matches your *Desired State*.

> **Observation:** If you delete a Pod, Kubernetes automatically creates a new one.  
> **Why?** Because the ReplicaSet controller’s only job is to ensure exactly 3 pods are running at all times.

---

## Understanding READY 1/1 vs READY 3/3
* **In `kubectl get pods`:** `1/1` means 1 out of 1 container is healthy inside that specific Pod.
* **In `kubectl get deploy`:** `3/3` means 3 out of 3 replicas (pods) are healthy across the cluster.

---

## Common Gotchas ⚠️
* **Typos:** Ensure `labels` and `matchLabels` are spelled correctly; otherwise, the Deployment can't "find" its Pods.
* **Container Exit:** Lightweight images like `busybox` will exit immediately unless you give them a task (e.g., `command: ["sh", "-c", "sleep 3600"]`).

