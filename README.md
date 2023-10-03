## Create EKS Cluster
```bash
eksctl create cluster --name fleetman --nodes-min=3 --region ap-northeast-2
```

## Change k8s cluster context
```bash
kubectl config get-contexts
kubectl config use-context <context-name>
kubectl config current-context
kubectl get all
```

## To configure kubectl for AWS EKS, you can use the AWS CLI's eks update-kubeconfig command. Replace <cluster-name> with the name of your EKS cluster and <region> with the AWS region where your cluster is located:
```bash
aws eks --region <region> update-kubeconfig --name <cluster-name>
```

## Cluster Commands
```bash
kubectl get nodes
kubectl cluster-info
```

## Namespace
```bash
kubectl get namespace
kubectl get ns --show-labels
kubectl get pods -n <namespace-name>
kubectl config set-context --current --namespace=<namespace-name>
kubectl delete ns <namespace-name>
```

## Pod Commands
```bash
kubectl get all
kubectl apply -f first-pod.yaml
kubectl get all
kubectl describe pod <pod-name>
kubectl exec <pod-name> ls
kubectl exec -it <pod-name> sh
wget http://localhost:80
ls
cat index.html
```

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

## Replicaset
```bash
$ kubectl get pods
$ kubectl get rs
$ kubectl describe rs <rs-name>
```
## Deployment
```bash
$ kubectl create deployment test --image=nginx
$ kubectl set image deployment test nginx=nginx:1.9.1 --record
$ kubectl rollout history deployment test

kubectl get pod <pod-name> -o yaml
kubectl get pod <pod-name> -o yaml > test-pod.yaml
```
deployments "test"
REVISION  CHANGE-CAUSE
1         <none>
2         kubectl set image deployment test nginx=nginx:1.9.1 --record=true
```bash
$ kubectl annotate deployment test kubernetes.io/change-cause="image updated to 1.9.1"
$ kubectl rollout undo deployment test
$ kubectl rollout undo deployment test --to-revision=2
$ kubectl rollout history deployment test
```
deployments "test"
REVISION  CHANGE-CAUSE
2         kubectl set image deployment test nginx=nginx:1.9.1 --record=true
3         <none>
```bash
kubectl scale deployment test --replicas=10
kubectl rollout pause deployment test
kubectl rollout resume deployment test
```
## Rolling Update
# Monitor the rolling update using the following command:
```bash
kubectl rollout status deployment webapp
```

# You can also check the rollout history to view the status of previous revisions
```bash
kubectl rollout history deployment webapp
```

# If, for any reason, you need to pause or rollback the update, you can use the kubectl rollout pause and kubectl rollout undo commands. For example, to pause the rollout
```bash
kubectl rollout pause deployment webapp
```

# And to undo the update and roll back to the previous version:
```bash
kubectl rollout undo deployment webapp
```
Remember to replace "webapp" with the name of your Deployment as specified in your YAML files