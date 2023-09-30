## Create EKS Cluster
```bash
eksctl create cluster --name fleetman --nodes-min=3 --region ap-northeast-2
```
## Commands
```bash
kubectl get all
kubectl get nodes
kubectl cluster-info

```
## Replicaset
```bash
$ kubectl get pods
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
