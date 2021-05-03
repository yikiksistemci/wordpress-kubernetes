# wordpress_kubernetes

### Deployment via kustomize

```bash
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
```


### Deployment via ArgoCD

- get admin password

```bash
$ kubectl-view-secret argocd-initial-admin-secret -n argocd
Choosing key: password
<your_password>
```

- login ArgoCD service
```bash
$ argocd login --server <your_server_url>
```

- create application
```bash
$ argocd app create wordpress --repo https://github.com/yikiksistemci/wordpress-kubernetes --path . --dest-namespace default --dest-server https://kubernetes.default.svc
application 'wordpress' created
$ argocd app list
NAME       CLUSTER                         NAMESPACE  PROJECT  STATUS     HEALTH   SYNCPOLICY  CONDITIONS  REPO                                                   PATH  TARGET
wordpress  https://kubernetes.default.svc  default    default  OutOfSync  Missing  <none>      <none>      https://github.com/yikiksistemci/wordpress-kubernetes  .     
```
