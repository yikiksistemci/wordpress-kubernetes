# wordpress_kubernetes

### Deployment via kustomize

<code>
$ cd wordpress-kubernetes

$ kubectl apply -k ./

namespace/wordpress created
secret/mysql-pass created
service/wordpress-mysql created
service/wordpress created
deployment.apps/wordpress-mysql created
deployment.apps/wordpress created
ingress.networking.k8s.io/wordpress-ingress created
persistentvolume/mysql-pv configured
persistentvolume/wordpress-pv configured
persistentvolumeclaim/mysql-pv-claim created
persistentvolumeclaim/wp-pv-claim created
</code>


### Deployment via ArgoCD

- get admin password

<code>
$  kubectl-view-secret argocd-initial-admin-secret -n argocd

Choosing key: password

<your_password>

</code>


- login ArgoCD service

<code>
$ argocd login --server <your_server_url>
</code>


- create application

<code>
$ argocd create
</code>
