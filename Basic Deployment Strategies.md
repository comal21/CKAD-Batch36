## Basic Deployment Strategies

### Task 1: Recreate Strategy in Kubernetes 
```
vi recreate.yaml
```
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: dep2
  name: dep2
spec:
  replicas: 2
  selector:
    matchLabels:
      app: dep2
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: dep2
    spec:
      containers:
      - image: nginx
        name: nginx
        ports:
        - containerPort: 80
```
```
kubectl apply -f recreate.yaml
```
Add a watch on the pods in a new tab
```
kubectl get po -w
```
Set a new image for the deployment
```
kubectl set image deploy dep2 nginx=nginx:latest --record
```
Check how the pods are getting deleted and recreated. 

Check the rollout history
```
kubectl rollout history deployment dep2
```

Cross check if the image has been updated by executing the below command
```
kubectl describe deployments.apps dep2
```
Now lets rollback. In the below command  if no revison number is given, it rollbacks to the immediate previous one.
```
kubectl rollout undo deployment dep2 --to-revision 1           
```
Check the history
```
kubectl rollout history deployment dep2
```


### Task 2: Rolling Update in Kubernetes 

```
vi dep.yaml 
```
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: dep1
  name: dep1
spec:
  replicas: 3
  selector:
    matchLabels:
      app: dep1
  strategy: {}
  template:
    metadata:
      labels:
        app: dep1
    spec:
      containers:
      - image: nginx
        name: nginx
        ports:
        - containerPort: 80
```

Note the strategy.Since nothing is given the default strategy comes into place which is the rolling update.

Apply the yaml file
```
kubectl apply -f dep.yaml
```
```
kubectl get deployments.apps,pod,rs
```

Note the Statgey and events by describing the deployment
```
kubectl describe deployments.apps dep1
```
Add a watch on the pods in a new tab
```
kubectl get po -w
```

Set a new image for the deployment
```
kubectl set image deploy dep1 nginx=nginx:latest --record
```
Check how the pods are getting deleted and recreated. 

Cross check if the image has been updated by executing the below command
```
kubectl describe deployments.apps dep1
```
To check the rollout history. The below command shows history of 10 versions.
```
kubectl rollout history deploy dep1
```
To Rollback
```
kubectl rollout undo deploy dep1 --to-revision 1
```
To check the history of a particular revision
```
kubectl rollout history deploy dep1 --revision=<revision-number>
```



