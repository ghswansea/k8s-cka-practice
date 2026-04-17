# Kubernetes Practice Project (CKA Preparation)

## Overview

This project contains Kubernetes manifests created during my preparation for the Certified Kubernetes Administrator (CKA) exam.
It demonstrates hands-on experience with application deployment, configuration management, and service exposure in Kubernetes.

---

## Tasks Implemented

* Deployed an application using **Deployment** and **Service**
* Configured **liveness** and **readiness probes**
* Used **ConfigMap** and **Secret** for configuration management
* Exposed services via **Ingress**
* Managed **resource requests and limits**

---

## Project Structure

```bash
k8s/
├── deployment.yaml
├── service.yaml
├── configmap.yaml
├── secret.yaml
└── ingress.yaml
```

---

## Deployment

Apply all resources:

```bash
kubectl apply -f k8s/
kubectl get all
```

---

## Access Application

### Option 1 — Port Forward (Working Solution)

```bash
kubectl port-forward svc/demo-service 8081:80
```

Open in browser:

```text
http://localhost:8081
```

---

### Option 2 — Ingress (Local Minikube)

Enable ingress:

```bash
minikube addons enable ingress
```

Update hosts file (inside WSL):

```text
127.0.0.1 demo.local
```

Test:

```bash
curl http://demo.local
```

Expected result:

* Nginx welcome page displayed

Check ingress controller:

```bash
kubectl get pods -n ingress-nginx
```

Check ingress resource:

```bash
kubectl get ingress
```

---

## Problem & Solution

### Problem

* Unable to access `http://demo.local` from Windows browser
* Root cause:

  * WSL and Windows use different network environments
  * Ingress exposed inside WSL is not directly reachable from Windows

---

### Solution

Used port-forward:

```bash
kubectl port-forward svc/demo-service 8081:80
```

Access:

```text
http://localhost:8081
```

---

### Why it works (mechanism)

* `kubectl` opens a **TCP tunnel via Kubernetes API server**

Traffic flow:

```text
Browser → localhost → kubectl → API server → Pod/Service
```

Bypasses:

* Ingress
* LoadBalancer
* Cluster networking limitations

---

## Result

* Application successfully accessible via local browser
* Demonstrated multiple Kubernetes access methods:

  * Ingress (domain-based access)
  * Port-forward (local debugging)

---

## Key Learnings

* Difference between **ClusterIP, Ingress, and port-forward**
* Kubernetes networking behavior in **WSL2 environment**
* Practical debugging of connectivity issues
* Separation of configuration using **ConfigMap and Secret**

---

## Next Steps (Upgrade Plan)

* Add **Horizontal Pod Autoscaler (HPA)**
* Package manifests using **Helm**
* Implement **CI/CD pipeline**
* Add monitoring (**Prometheus + Grafana**)

---

## Purpose

This project supports my transition into **Cloud/DevOps engineering**, focusing on Kubernetes-based deployment and operations.
