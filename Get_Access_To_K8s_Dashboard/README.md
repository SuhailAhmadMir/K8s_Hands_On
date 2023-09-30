# Accessing the Kubernetes Dashboard on Amazon EKS Cluster

This guide will walk you through the steps to access the Kubernetes Dashboard on an Amazon Elastic Kubernetes Service (EKS) cluster. The Kubernetes Dashboard provides a graphical interface to manage and monitor your cluster.

## Prerequisites

- Ensure that you have `kubectl` installed on your local machine and properly configured to connect to your EKS cluster. You should have the necessary IAM permissions to access the cluster.

## Deploy the Kubernetes Dashboard

Deploy the Kubernetes Dashboard by applying the official YAML manifest provided by Kubernetes. Run the following command:

   ```bash
   kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.4.0/aio/deploy/recommended.yaml
   ```
This command deploys the necessary resources for the Kubernetes Dashboard.

## Create an Admin User (Service Account)
To access the Dashboard securely, you need to create a Service Account and ClusterRoleBinding with admin privileges. Below is an example YAML manifest that accomplishes this:

```bash
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
---
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
Save this YAML to a file, e.g., admin-user.yaml, and apply it to your cluster:

```bash
kubectl apply -f admin-user.yaml
```
This command creates the admin Service Account and binds it with the necessary permissions.

## Get an Authentication Token
To log in to the Dashboard, you need an authentication token. Retrieve the token using the following command:

```bash
kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')
```
This command retrieves the token from the secret associated with the admin-user Service Account.

## Access the Dashboard
Now that you have the token, you can access the Dashboard by running the following command:

```bash
kubectl proxy
```

This command starts a local proxy to your cluster. You can access the Dashboard at the following URL:

http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/

Ensure you select the "Token" option when prompted for authentication, and enter the token you obtained earlier.

Note: The Kubernetes Dashboard provides a graphical interface to your cluster. Use it cautiously, especially in production environments. Follow security best practices and restrict access as needed to maintain the security of your EKS cluster.

=================================================================================================================================================

### Installing Kubernetes dashboard using Helm
# Create the kubernetes-dashboard namespace
kubectl create namespace kubernetes-dashboard

# Add the Helm repository for the Kubernetes Dashboard (if not added already)
helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/

# Update the Helm repositories
helm repo update

# Deploy the Kubernetes Dashboard using Helm with auto-generated release name
helm install --generate-name kubernetes-dashboard/kubernetes-dashboard --namespace=kubernetes-dashboard


