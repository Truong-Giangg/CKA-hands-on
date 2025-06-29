
## 🧱 Cluster Architecture, Installation & Configuration (4 Questions)

### 1. Verify Node Kubernetes Version
**Question:**  
Check and print the current Kubernetes version running on node `cka-node1`.

**Answer:**  
```bash
kubectl get node cka-node1 -o jsonpath='{.status.nodeInfo.kubeletVersion}'
```

---

### 2. Upgrade Node Kubernetes Version
**Question:**  
Upgrade the node `cka-node2` to Kubernetes version `v1.32.2`.

**Answer:**  
```bash
kubectl drain cka-node2 --ignore-daemonsets
sudo apt install kubelet=1.32.2-00 kubeadm=1.32.2-00
sudo kubeadm upgrade node
kubectl uncordon cka-node2
```

---

### 3. Find Kubelet Config File
**Question:**  
Find and output the path of the kubelet configuration file.

**Answer:**  
```bash
systemctl status kubelet | grep config
# Typically outputs: /var/lib/kubelet/config.yaml
```

---

### 4. Static Pod Verification
**Question:**  
Verify if control-plane components are running as static pods.

**Answer:**  
```bash
ls /etc/kubernetes/manifests/
# Look for etcd.yaml, kube-apiserver.yaml, etc.
```

---

## 🛠 Troubleshooting (5 Questions)

### 5. Pod in Pending State
**Question:**  
Investigate why Pod `myapp` is stuck in a Pending state.

**Answer:**  
```bash
kubectl describe pod myapp
# Look for nodeSelector, imagePull, or scheduling issues.
```

---

### 6. Node NotReady
**Question:**  
Node `worker-1` is showing status NotReady. Identify the issue.

**Answer:**  
```bash
kubectl describe node worker-1
journalctl -u kubelet -n 100
```

---

### 7. Inter-Pod Connectivity
**Question:**  
Verify network connectivity between Pod A and Pod B on different nodes.

**Answer:**  
```bash
kubectl exec -it pod-a -- ping <pod-b-ip>
```

---

### 8. Service Has No Endpoints
**Question:**  
The Service `backend-svc` has no endpoints. Identify the root cause.

**Answer:**  
```bash
kubectl describe svc backend-svc
kubectl get pods -l app=backend -o wide
```

---

### 9. Kube-Scheduler Health
**Question:**  
Check whether `kube-scheduler` is running properly on the control plane.

**Answer:**  
```bash
kubectl get pods -n kube-system | grep kube-scheduler
kubectl logs <scheduler-pod> -n kube-system
```

---

## 📦 Workloads & Scheduling (3 Questions)

### 10. Create Deployment
**Question:**  
Create a Deployment called `web-app` with 3 replicas using the image `nginx:latest`.

**Answer:**  
```bash
kubectl create deployment web-app --image=nginx:latest --replicas=3
```

---

### 11. NodeSelector Scheduling
**Question:**  
Create a Pod that runs only on nodes with the label `disktype=ssd`.

**Answer:**  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-on-ssd
spec:
  containers:
    - name: app
      image: nginx
  nodeSelector:
    disktype: ssd
```

---

### 12. Rolling Update of Deployment
**Question:**  
Perform a rolling update to change the image of `web-app` Deployment to `nginx:1.25`.

**Answer:**  
```bash
kubectl set image deployment/web-app nginx=nginx:1.25
```

---

## 🌐 Services & Networking (3 Questions)

### 13. Create NodePort Service
**Question:**  
Expose Pod `nginx-pod` on port `8080` using a Service of type NodePort.

**Answer:**  
```bash
kubectl expose pod nginx-pod --type=NodePort --port=8080
```

---

### 14. NetworkPolicy Ingress
**Question:**  
Create a NetworkPolicy to allow ingress only from Pods labeled `access=true`.

**Answer:**  
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-specific-ingress
spec:
  podSelector: {}
  ingress:
    - from:
        - podSelector:
            matchLabels:
              access: "true"
```

---

### 15. Get DNS Cluster IP
**Question:**  
Write the cluster DNS IP to `/tmp/dns-ip.txt`.

**Answer:**  
```bash
kubectl get svc kube-dns -n kube-system -o jsonpath='{.spec.clusterIP}' > /tmp/dns-ip.txt
```

---

## 📁 Storage (2 Questions)

### 16. Create a Local PersistentVolume
**Question:**  
Create a PersistentVolume named `pv-local` using hostPath `/mnt/data` and size `10Gi`.

**Answer:**  
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-local
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
```

---

### 17. Attach PVC to a Pod
**Question:**  
Attach PVC `pvc-app` to a Pod named `storage-app`.

**Answer:**  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: storage-app
spec:
  containers:
    - name: app
      image: nginx
      volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: app-volume
  volumes:
    - name: app-volume
      persistentVolumeClaim:
        claimName: pvc-app
```
