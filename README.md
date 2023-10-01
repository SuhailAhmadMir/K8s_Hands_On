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
kubectl get pods -n my-namespace
kubectl config set-context --current --namespace=<namespace>
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
cat index.html

```

# In pod check the node on which pod is deployed using
```bash
kubectl describe pod <pod-name>
```
# Extract pod name and check public ip of node on aws and use that public ip with 30080 port in browser
# <puclic-ip-of-node>:30080

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
