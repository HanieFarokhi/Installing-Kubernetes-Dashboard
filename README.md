# Installing-Kubernetes-Dashboard
Installing-Kubernetes-Dashboard

## Deploying Dashboard

# Update your system:
sudo apt update
sudo apt upgrade -y

# Install Minikube (if you don't have a Kubernetes cluster):
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
minikube start --driver=none

# Install kubectl:
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl


# Deploy Kubernetes Dashboard:
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml

# Create a Service Account and ClusterRoleBinding:
Create a YAML file named dashboard-adminuser.yaml with the following
content:

```yaml
  apiVersion: v1 
  kind: ServiceAccount
    metadata:
    name: admin-user
    namespace: kubernetes-dashboard
  apiVersion: rbac.authorization.k8s.io/v1
  kind: ClusterRoleBinding
  metadata:
    name: admin-user
  roleRef:
    apiGroup: rbac.authorization.k8s.io
    kind: ClusterRole
    name: cluster-admin
  subjects:
  - kind: ServiceAccount
    name: admin-user
    namespace: kubernetes-dashboard
```


# Apply the configuration:
 kubectl apply -f dashboard-adminuser.yaml


# 	Obtain the Bearer Token:
  kubectl -n kubernetes-dashboard create token admin-user


# 	Start the Kubernetes Proxy:
  kubectl proxy


## 	Access the Kubernetes Dashboard: Open a web browser and navigate to:
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
You will be prompted to enter a token. Use the token obtained in the previous step.


## Verifying Installation:
# 	Check the Dashboard Deployment and Service:
kubectl get deployments -n kubernetes-dashboard
kubectl get services -n kubernetes-dashboard

# 	Check Dashboard Pod Status:
kubectl get pods -n kubernetes-dashboard

# 	Confirm Proxy is Running:
curl http://localhost:8001



### Step 2: Deploying Cacti with MariaDB Integration
Once MariaDB is deployed and you've created a new database and user passwords, you can proceed with deploying Cacti using the following stack YAML configuration:









