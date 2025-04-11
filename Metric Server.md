## Metrics Server

Install Metrics Server 

```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```
OR 
```
helm repo add metrics-server https://kubernetes-sigs.github.io/metrics-server
```
```
helm install metrics-server metrics-server/metrics-server --namespace kube-system --create-namespace
```
Check the status of the metrics server
```
kubectl -n kube-system get po
```
Download the patch for the Mtrics Server
```
wget https://gist.githubusercontent.com/initcron/1a2bd25353e1faa22a0ad41ad1c01b62/raw/008e23f9fbf4d7e2cf79df1dd008de2f1db62a10/k8s-metrics-server.patch.yaml
```
Apply the patch
```
kubectl patch deploy metrics-server -p "$(cat k8s-metrics-server.patch.yaml)" -n kube-system
```
```
kubectl -n kube-system get po
```
To check the resource utilization by nodes
```
kubectl top node
```
To check the resource utilization by pods
```
kubectl top pod
```
Sort the output on CPU Utilization
```
kubectl top pod -A --sort-by cpu
```
Sort the output on Memory Utilization
```
kubectl top pod -A --sort-by memory
```
To check the total utilization by all pods
```
kubectl top pod --sum=true -A
```
To check all the options
```
kubectl top pod --help
```
### CleanUp:
If you deployed the Metrics Server with Helm, you can use:
```
helm uninstall metrics-server -n kube-system
```
OR
Delete the Metrics Server deployment:
```
kubectl delete deployment metrics-server -n kube-system
```
Remove any associated resources: In some cases, additional resources like ServiceAccount, ClusterRole, ClusterRoleBinding, or Service may also have been created. To clean up these resources, you can use:
```
kubectl delete service metrics-server -n kube-system
kubectl delete apiservice v1beta1.metrics.k8s.io
kubectl delete clusterrolebinding metrics-server:system:auth-delegator
kubectl delete clusterrole metrics-server:system:auth-delegator
kubectl delete rolebinding metrics-server-auth-reader -n kube-system
kubectl delete role metrics-server-auth-reader -n kube-system
```

