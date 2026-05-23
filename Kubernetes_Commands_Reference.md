# Kubernetes Deployment Project - Commands Reference

## 1. Install Kubernetes Tools

### macOS

```bash
brew install minikube
brew install kubectl
```

---

# 2. Start Kubernetes Cluster

```bash
minikube start --driver=docker
```

---

# 3. Verify Cluster

```bash
kubectl cluster-info

kubectl get nodes
```

---

# 4. Create Application Project

```bash
mkdir k8s-webapp
cd k8s-webapp
```

---

# 5. Install Node.js Dependencies

```bash
npm install
```

---

# 6. Build Docker Image

```bash
docker build -t YOUR_DOCKERHUB_USERNAME/k8s-webapp:v1 .
```

---

# 7. Login to Docker Hub

```bash
docker login
```

---

# 8. Push Docker Image

```bash
docker push YOUR_DOCKERHUB_USERNAME/k8s-webapp:v1
```

---

# 9. Create Kubernetes Namespace

```bash
kubectl create namespace webapp
```

---

# 10. Apply Configurations

## Apply ConfigMap

```bash
kubectl apply -f configmap.yaml
```

## Apply Secret

```bash
kubectl apply -f secret.yaml
```

## Apply Persistent Volume

```bash
kubectl apply -f pv.yaml
```

## Apply Persistent Volume Claim

```bash
kubectl apply -f pvc.yaml
```

## Apply Deployment

```bash
kubectl apply -f deployment.yaml
```

## Apply Service

```bash
kubectl apply -f service.yaml
```

## Apply Ingress

```bash
kubectl apply -f ingress.yaml
```

---

# 11. Enable Minikube Addons

## Enable Ingress Controller

```bash
minikube addons enable ingress
```

## Enable Metrics Server

```bash
minikube addons enable metrics-server
```

---

# 12. Update Hosts File

```bash
sudo nano /etc/hosts
```

Add:

```text
$(minikube ip) webapp.local
```

---

# 13. Test Application

## Browser

```text
http://webapp.local
```

## API Test

```bash
curl http://webapp.local
```

---

# 14. Test Persistent Storage

## Write Data

```bash
curl http://webapp.local/write
```

## Read Data

```bash
curl http://webapp.local/read
```

---

# 15. Configure Horizontal Pod Autoscaler

```bash
kubectl autoscale deployment webapp-deployment \
  --cpu-percent=50 \
  --min=2 \
  --max=5 \
  -n webapp
```

---

# 16. Verify HPA

```bash
kubectl get hpa -n webapp
```

---

# 17. Generate Load Traffic

```bash
kubectl run -i --tty load-generator \
  --rm \
  --image=busybox \
  --restart=Never \
  -- /bin/sh
```

Inside the shell:

```bash
while true; do wget -q -O- http://webapp-service.webapp.svc.cluster.local; done
```

---

# 18. Monitor Scaling

```bash
kubectl get pods -n webapp -w
```

---

# 19. Monitoring and Troubleshooting

## View Pods

```bash
kubectl get pods -n webapp
```

## View Logs

```bash
kubectl logs deployment/webapp-deployment -n webapp
```

## Resource Usage

```bash
kubectl top pods -n webapp
```

## Describe Deployment

```bash
kubectl describe deployment webapp-deployment -n webapp
```

## Check Cluster Events

```bash
kubectl get events -n webapp
```

---

# 20. Verify All Resources

```bash
kubectl get all -n webapp
```

---

# 21. Useful Docker Commands

## View Images

```bash
docker images
```

## View Running Containers

```bash
docker ps
```

## Stop Container

```bash
docker stop <container-id>
```

## Remove Container

```bash
docker rm <container-id>
```

---

# 22. Useful Kubernetes Cleanup Commands

## Delete Namespace

```bash
kubectl delete namespace webapp
```

## Stop Minikube

```bash
minikube stop
```

## Delete Minikube Cluster

```bash
minikube delete
```

---

# End of Commands Reference
