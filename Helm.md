## Helm

### Commonly used Helm Commands

Download the shell script for the installation of the Helm package manager and run it.
```
wget  https://s3.ap-south-1.amazonaws.com/files.cloudthat.training/devops/kubernetes-essentials/helm.sh
```
```
bash helm.sh
```
To check the version of Helm installed
```
helm version
```



To add a Helm chart repository to your local environment. Below we are adding two Helm Chart Repositories.
```
helm repo add bitnami https://charts.bitnami.com/bitnami 
```
```
helm repo add stable-charts https://charts.helm.sh/stable
```
To list the Helm chart repositories configured in your local environment.
```
helm repo list
```



To search for Helm charts related to WordPress in the configured Helm repositories
```
helm search repo wordpress
```

To install the WordPress application using the Bitnami Helm chart with the release name my-wordpress
```
helm install my-wordpress bitnami/wordpress \
  --set mariadb.primary.persistence.enabled=false \
  --set persistence.enabled=false \
  --set service.type=NodePort
```

To list all the releases currently installed on your Kubernetes cluster
```
helm list
```

You can install multiple helm charts from the same repo, but with unique name
```
helm install app-wordpress bitnami/wordpress
```


To uninstall the WordPress application deployed using Helm with the release name my-wordpress
```
helm uninstall app-wordpress
```

To upgrade(update) an existing chart
```
helm upgrade my-wordpress bitnami/wordpress
```
Notice the revision number

If you want to edit the existing charts, download and exract the chart. To download the Bitnami WordPress Helm chart to your local machine.
```
helm fetch bitnami/wordpress
```
```
tar -xvzf wordpress-23.1.14.tgz
```

### Wordpress 
In the above steps we have already installed wordpress

Verify that the pods are running.
```
kubectl get pods
```
Now Notice that MariaDB (database) pods are part of statefulset.
```
kubectl get sts
```
```
kubectl describe sts
```
Also Notice that the front end of wordpress are part of deployment
```
kubectl get deploy
```
View the services using the below commands. 
```
kubectl get all
```
```
kubectl get svc my-wordpress
```

Open the browser and paste any worker node's Ip : The nodeport number of the service noted on the previous step. Observe that the WordPress site is up and running.

Add `/admin` at the end of the URL to be directed to the login page

Execute the below commands to get the user id and password. USe the same to log in to the Wordpress application that you have just deployed
```
echo Username: user
```
```
echo Password: $(kubectl get secret --namespace default wordpress -o jsonpath="{.data.wordpress-password}" | base64 -d)
```
To remove a Helm repository from your local Helm client configuration
```
helm uninstall my-wordpress
```
```
helm repo remove stable-charts
```
```
helm repo remove bitnami
```
To remove Helm package manager
```
sudo apt-get remove helm
```


 
 
