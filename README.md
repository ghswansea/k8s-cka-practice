# k8s-cka-practice project

# Tasks:
## Deployed an application using Deployment and Service
## Configured liveness/readiness probes
## Used ConfigMap and Secret for configuration
## Exposed services via Ingress
## Managed resource limits

# Actions:

## My kubernetes project for practice
k8s/ folder include 
deployment.yaml, 
service.yaml, 
configmap.yaml,
secret.yaml,
ingress.yaml

kubectl apply -f k8s/
kubectl get all
## For Ingress to work locally (Minikube):
minikube addons enable ingress
## Then add to /etc/hosts:
127.0.0.1 demo.local
## Check
curl http://demo.local => display ngnix welcome page
kubectl get pods -n ingress-nginx 
=> you should see a pod like: ingress-nginx-controller-xxxxx status running
kubectl get ingress

## Problem & solution
problem: can not open http://demo.local from windows browser
solution:
    Executed command:
    kubectl port-forward svc/demo-service 8081:80
    This creates a tunnel:
    localhost:8081 → Kubernetes service:80
    Then accessed application via:
    http://localhost:8081
It works because: 
        Pattern (Why it works — mechanism, not theory)
        1. kubectl opens a TCP tunnel via Kubernetes API server
        2. Traffic flow:
           Browser → localhost → kubectl → API server → Pod/Service
        3. It bypasses:
            Ingress
            LoadBalancer
            Cluster networking restrictions

# Result: Ingress works when you can reach your service through a domain (demo.local) instead of directly via IP or NodePort.

# Next step - upgrade
Add HPA
Add Helm chart
Add CI/CD