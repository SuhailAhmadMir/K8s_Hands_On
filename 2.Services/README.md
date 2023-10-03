# In pod check the node on which pod is deployed using
```bash
kubectl describe pod <pod-name>
```
# To access a specific pod and determine the public IP of the node it's deployed on, follow these steps:
- Extract Pod Name: First, identify the name of the pod you want to access within your Kubernetes cluster.

- Check Public IP of Node: Use the following command to check the public IP address of the node where the pod is deployed:
```bash
kubectl get pods -o wide
```
# Access the Pod: Construct the URL using the public IP address of the node and port 30080. You can access the pod using the following format:
```bash
http://<public-ip-of-node>:30080
```