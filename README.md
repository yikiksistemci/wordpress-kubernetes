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
- sync repo 

```bash

$ argocd app sync wordpress

TIMESTAMP                  GROUP                    KIND              NAMESPACE                  NAME    STATUS    HEALTH        HOOK  MESSAGE
2021-05-04T00:28:49+03:00                         Secret              wordpress            mysql-pass  OutOfSync  Missing
2021-05-04T00:28:49+03:00   apps              Deployment              wordpress             wordpress  OutOfSync  Missing
2021-05-04T00:28:49+03:00                      Namespace                                    wordpress  OutOfSync  Missing
2021-05-04T00:28:49+03:00                     PersistentVolume                           wordpress-pv  OutOfSync  Missing
2021-05-04T00:28:49+03:00                     PersistentVolumeClaim   wordpress        mysql-pv-claim  OutOfSync  Missing
2021-05-04T00:28:49+03:00                        Service              wordpress       wordpress-mysql  OutOfSync  Missing
2021-05-04T00:28:49+03:00   apps              Deployment              wordpress       wordpress-mysql  OutOfSync  Missing
2021-05-04T00:28:49+03:00  networking.k8s.io     Ingress              wordpress     wordpress-ingress  OutOfSync  Missing
2021-05-04T00:28:49+03:00                     PersistentVolume                               mysql-pv  OutOfSync  Missing
2021-05-04T00:28:49+03:00                     PersistentVolumeClaim   wordpress           wp-pv-claim  OutOfSync  Missing
2021-05-04T00:28:49+03:00                        Service              wordpress             wordpress  OutOfSync  Missing
2021-05-04T00:28:53+03:00          Namespace                         wordpress    Synced  Missing
2021-05-04T00:28:53+03:00             Secret   wordpress            mysql-pass    Synced  Missing
2021-05-04T00:28:54+03:00         PersistentVolume                      wordpress-pv    Synced  Missing
2021-05-04T00:28:54+03:00         PersistentVolume                          mysql-pv    Synced  Missing
2021-05-04T00:28:54+03:00         PersistentVolumeClaim   wordpress        mysql-pv-claim    Synced  Progressing
2021-05-04T00:28:54+03:00         PersistentVolumeClaim   wordpress           wp-pv-claim    Synced  Progressing
2021-05-04T00:28:54+03:00         PersistentVolumeClaim   wordpress        mysql-pv-claim    Synced  Healthy
2021-05-04T00:28:54+03:00         PersistentVolumeClaim   wordpress           wp-pv-claim    Synced  Healthy
2021-05-04T00:28:55+03:00            Service   wordpress             wordpress    Synced  Healthy
2021-05-04T00:28:55+03:00            Service   wordpress       wordpress-mysql    Synced  Healthy
2021-05-04T00:28:55+03:00   apps  Deployment   wordpress       wordpress-mysql    Synced  Progressing
2021-05-04T00:28:55+03:00   apps  Deployment   wordpress             wordpress    Synced  Progressing
2021-05-04T00:28:55+03:00                        Service              wordpress       wordpress-mysql    Synced   Healthy                  service/wordpress-mysql created
2021-05-04T00:28:55+03:00                        Service              wordpress             wordpress    Synced   Healthy                  service/wordpress created
2021-05-04T00:28:55+03:00  networking.k8s.io     Ingress              wordpress     wordpress-ingress  OutOfSync  Missing                  ingress.networking.k8s.io/wordpress-ingress created
2021-05-04T00:28:55+03:00                         Secret              wordpress            mysql-pass    Synced   Missing                  secret/mysql-pass created
2021-05-04T00:28:55+03:00                     PersistentVolume          default          wordpress-pv   Running    Synced                  persistentvolume/wordpress-pv created
2021-05-04T00:28:55+03:00                     PersistentVolumeClaim   wordpress        mysql-pv-claim    Synced   Healthy                  persistentvolumeclaim/mysql-pv-claim created
2021-05-04T00:28:55+03:00   apps              Deployment              wordpress       wordpress-mysql    Synced   Progressing              deployment.apps/wordpress-mysql created
2021-05-04T00:28:55+03:00   apps              Deployment              wordpress             wordpress    Synced   Progressing              deployment.apps/wordpress created
2021-05-04T00:28:55+03:00                      Namespace                default             wordpress   Running    Synced                  namespace/wordpress created
2021-05-04T00:28:55+03:00                     PersistentVolume          default              mysql-pv   Running    Synced                  persistentvolume/mysql-pv created
2021-05-04T00:28:55+03:00                     PersistentVolumeClaim   wordpress           wp-pv-claim    Synced   Healthy                  persistentvolumeclaim/wp-pv-claim created
2021-05-04T00:28:55+03:00  networking.k8s.io     Ingress   wordpress     wordpress-ingress    Synced  Progressing              ingress.networking.k8s.io/wordpress-ingress created

Name:               wordpress
Project:            default
Server:             https://kubernetes.default.svc
Namespace:          default
URL:                https://192.168.1.22:30649/applications/wordpress
Repo:               https://github.com/yikiksistemci/wordpress-kubernetes
Target:
Path:               .
SyncWindow:         Sync Allowed
Sync Policy:        <none>
Sync Status:        Synced to  (2f601a3)
Health Status:      Progressing

Operation:          Sync
Sync Revision:      2f601a3abb593c8c12335304a1880fb10444d4a9
Phase:              Succeeded
Start:              2021-05-04 00:28:49 +0300 +03
Finished:           2021-05-04 00:28:55 +0300 +03
Duration:           6s
Message:            successfully synced (all tasks run)

GROUP              KIND                   NAMESPACE  NAME               STATUS   HEALTH       HOOK  MESSAGE
                   Namespace              default    wordpress          Running  Synced             namespace/wordpress created
                   Secret                 wordpress  mysql-pass         Synced                      secret/mysql-pass created
                   PersistentVolume       default    wordpress-pv       Running  Synced             persistentvolume/wordpress-pv created
                   PersistentVolume       default    mysql-pv           Running  Synced             persistentvolume/mysql-pv created
                   PersistentVolumeClaim  wordpress  mysql-pv-claim     Synced   Healthy            persistentvolumeclaim/mysql-pv-claim created
                   PersistentVolumeClaim  wordpress  wp-pv-claim        Synced   Healthy            persistentvolumeclaim/wp-pv-claim created
                   Service                wordpress  wordpress-mysql    Synced   Healthy            service/wordpress-mysql created
                   Service                wordpress  wordpress          Synced   Healthy            service/wordpress created
apps               Deployment             wordpress  wordpress-mysql    Synced   Progressing        deployment.apps/wordpress-mysql created
apps               Deployment             wordpress  wordpress          Synced   Progressing        deployment.apps/wordpress created
networking.k8s.io  Ingress                wordpress  wordpress-ingress  Synced   Progressing        ingress.networking.k8s.io/wordpress-ingress created
                   Namespace                         wordpress          Synced
                   PersistentVolume                  mysql-pv           Synced
                   PersistentVolume                  wordpress-pv       Synced


```

- get app details

```bash
$ argocd app get wordpress
Name:               wordpress
Project:            default
Server:             https://kubernetes.default.svc
Namespace:          default
URL:                https://192.168.1.22:30649/applications/wordpress
Repo:               https://github.com/yikiksistemci/wordpress-kubernetes
Target:             
Path:               .
SyncWindow:         Sync Allowed
Sync Policy:        <none>
Sync Status:        Synced to  (2f601a3)
Health Status:      Healthy

GROUP              KIND                   NAMESPACE  NAME               STATUS   HEALTH   HOOK  MESSAGE
                   Namespace              default    wordpress          Running  Synced         namespace/wordpress created
                   Secret                 wordpress  mysql-pass         Synced                  secret/mysql-pass created
                   PersistentVolume       default    wordpress-pv       Running  Synced         persistentvolume/wordpress-pv created
                   PersistentVolume       default    mysql-pv           Running  Synced         persistentvolume/mysql-pv created
                   PersistentVolumeClaim  wordpress  mysql-pv-claim     Synced   Healthy        persistentvolumeclaim/mysql-pv-claim created
                   PersistentVolumeClaim  wordpress  wp-pv-claim        Synced   Healthy        persistentvolumeclaim/wp-pv-claim created
                   Service                wordpress  wordpress-mysql    Synced   Healthy        service/wordpress-mysql created
                   Service                wordpress  wordpress          Synced   Healthy        service/wordpress created
apps               Deployment             wordpress  wordpress-mysql    Synced   Healthy        deployment.apps/wordpress-mysql created
apps               Deployment             wordpress  wordpress          Synced   Healthy        deployment.apps/wordpress created
networking.k8s.io  Ingress                wordpress  wordpress-ingress  Synced   Healthy        ingress.networking.k8s.io/wordpress-ingress created
                   Namespace                         wordpress          Synced                  
                   PersistentVolume                  mysql-pv           Synced                  
                   PersistentVolume                  wordpress-pv       Synced 

```
