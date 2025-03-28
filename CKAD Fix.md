If you get the below `error` when running kubectl commands, execute the command given below.

`The connection to the server localhost:8080 was refused - did you specify the right host or port?`
```
sudo su
```
```
export KUBECONFIG=/etc/kubernetes/admin.conf
```

`/etc/kubernetes/admin.conf` is configuration file used in a Kubernetes cluster. It contains the necessary information for authenticating and authorizing users with the Kubernetes API server
* `Purpose`: This file is generated during the initialization of a Kubernetes cluster using kubeadm. It contains credentials for the Kubernetes cluster admin user (often called "admin").
* `Contents`: The file includes configuration data like the cluster's API server address, a client certificate, a client key, and a kubeconfig file format.
* `Usage`: This file is typically used by the cluster administrator to manage the cluster. When you execute kubectl commands with administrator privileges, kubectl uses the credentials from admin.conf unless another kubeconfig file is specified.

