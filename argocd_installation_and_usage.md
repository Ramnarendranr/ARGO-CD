You can install Argocd in 3 ways:

1. Plain manifest
2. Helm
3. Operator

Installing argocd using plain manifest:

install docker and open docker daemon(start the docker app)
start minikube before installing argocd.
**minikube start**
**minikube status**


now go to argo cd docs and get the command to install argocd:
**kubectl create namespace argocd**
**kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml**

now check whther the pods are in running state:
**kubectl get pods -n argocd**

            NAME                                                READY   STATUS    RESTARTS   AGE
            argocd-application-controller-0                     1/1     Running   0          36s
            argocd-applicationset-controller-64c77cff5d-8scr7   1/1     Running   0          37s
            argocd-dex-server-5fb9857f6c-mjcdq                  1/1     Running   0          37s
            argocd-notifications-controller-679d59bf4b-c6cst    1/1     Running   0          37s
            argocd-redis-66d9777b78-jwcmm                       1/1     Running   0          37s
            argocd-repo-server-554bd7bf5-sh4lb                  1/1     Running   0          36s
            argocd-server-6697c5749b-t9tq5                      1/1     Running   0          36s

            You can see all the components from argocd architecture, if you run the get pods command.

now you have to change the type from cluster ip to Nodeport mode:
**kubectl get svc**
**kubectl get svc -n argocd**
**kubectl edit svc argocd-server -n argocd**

Now get the ip address to connect to the internet:
**minikube service list -n argocd**
        |-----------|-----------------------------------------|--------------|-----|
        | NAMESPACE |                  NAME                   | TARGET PORT  | URL |
        |-----------|-----------------------------------------|--------------|-----|
        | argocd    | argocd-applicationset-controller        | No node port |     |
        | argocd    | argocd-dex-server                       | No node port |     |
        | argocd    | argocd-metrics                          | No node port |     |
        | argocd    | argocd-notifications-controller-metrics | No node port |     |
        | argocd    | argocd-redis                            | No node port |     |
        | argocd    | argocd-repo-server                      | No node port |     |
        | argocd    | argocd-server                           | http/80      |     |
        |           |                                         | https/443    |     |
        | argocd    | argocd-server-metrics                   | No node port |     |
        |-----------|-----------------------------------------|--------------|-----|

**minikube service argocd-server -n argocd**
        |-----------|---------------|-------------|---------------------------|
        | NAMESPACE |     NAME      | TARGET PORT |            URL            |
        |-----------|---------------|-------------|---------------------------|
        | argocd    | argocd-server | http/80     | http://192.168.49.2:31305 |
        |           |               | https/443   | http://192.168.49.2:32701 |
        |-----------|---------------|-------------|---------------------------|
        🏃  Starting tunnel for service argocd-server.
        |-----------|---------------|-------------|------------------------|
        | NAMESPACE |     NAME      | TARGET PORT |          URL           |
        |-----------|---------------|-------------|------------------------|
        | argocd    | argocd-server |             | http://127.0.0.1:51053 |
        |           |               |             | http://127.0.0.1:51054 |
        |-----------|---------------|-------------|------------------------|
        [argocd argocd-server  http://127.0.0.1:51053
        http://127.0.0.1:51054]
        ❗  Because you are using a Docker driver on darwin, the terminal needs to be open to run it.


use any of the two ips to login to argo cd
http://127.0.0.1:51053 |
http://127.0.0.1:51054 |

once you're there in argo cd, it will ask for username password
**username: admin**

to get password:
**kubectl get secret -n argocd**
            NAME                          TYPE     DATA   AGE
            argocd-initial-admin-secret   Opaque   1      7m15s
            argocd-notifications-secret   Opaque   0      7m31s
            argocd-secret                 Opaque   5      7m31s

**kubectl edit secret argocd-initial-admin-secret -n argocd**

**echo TlhLY3JhcFh0aE82a29jNA== | base64 --decode**
**password: NXKcrapXthO6koc4%**