## **Project Overview**  
This project demonstrates how to deploy an **Nginx** web server on Kubernetes using:  
- A **Deployment** to manage pods  
- A **NodePort Service** to expose the application  
- A custom **namespace (`nginx-namespace`)** for isolation  

---

## **Prerequisites**  
- Kubernetes cluster (Minikube, EKS, AKS, GKE, etc.)  
- `kubectl` configured  

---

## **Steps to Deploy**  

### **1. Create a Custom Namespace**  
```bash
kubectl create namespace nginx-namespace
```  
kubectl get namespace 

---

### **2. Apply the Deployment**  
Deploys **2 Nginx pods** in `nginx-namespace`:  
```bash
kubectl apply -f deployment.yml
```  
**Verify:**  
```bash
kubectl get pods -n nginx-namespace
```  

---

### **3. Expose the Deployment via NodePort Service**  
Creates a **Service** to access Nginx externally:  
```bash
kubectl apply -f service.yml
```  
**Check Service & Assigned Port:**  
```bash
kubectl get svc -n nginx-namespace
```  
Example output:  
```
NAME            TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
myapp-service   NodePort   10.96.123.45   <none>        80:30388/TCP   10s
```  
👉 **Access Nginx at:** `http://192.168.59.104:30388`  

*(For Minikube: Run `minikube service myapp-service -n nginx-namespace` to auto-open the URL.)*  

---

## **Key Files**  

### **1. `deployment.yml`**  
- Manages **2 replicas** of Nginx pods.  
- Uses **`nginx-namespace`** for isolation.  
- Exposes **port 80** inside containers.  

### **2. `service.yml`**  
- Creates a **NodePort Service** mapping:  
  - Service port `80` → Pod port `80`
  - kubectl assign port number : 30388
- Automatically assigns a high-range port (e.g., `30000-32767`).  

---

## **Cleanup**  
Delete all resources:  
```bash
kubectl delete -f deployment.yml -f service.yml
kubectl delete namespace nginx-namespace
```  

---

## **Why This Setup?**  
✅ **Isolation**: Uses a dedicated namespace (`nginx-namespace`).  
✅ **Scalability**: Deployment ensures **2 pods** for redundancy.  
✅ **Accessibility**: NodePort Service allows external traffic.  

---
