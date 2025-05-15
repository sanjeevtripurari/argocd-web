
# webquote - ArgoCD Demo Application

`webquote` is a Kubernetes-based demo project designed to showcase GitOps deployments using [ArgoCD](https://argo-cd.readthedocs.io). It features a dynamic quote-rendering webpage served by NGINX, with language and color customizations applied through Kubernetes jobs and config maps.

## 📦 Project Structure

```
argocd-web
├── README.md
└── webquote
    ├── argocd-webquote.yaml
    ├── nginx-deployment.yaml
    └── web-content-writer.yaml

```
This repository contains Kubernetes manifests for the following components:

- `nginx-deployment.yaml`:  
  Deploys a lightweight `nginx:alpine` pod and exposes it via a NodePort service on port `30081`.

- `web-content-writer.yaml`:  
  Includes:
  - A `ConfigMap` named `page-config` containing:
    - Default language
    - Background color
  - A `Job` named `content-writer` that:
    - Installs `fortune` and generates a random quote
    - Creates a multilingual HTML page with a `<select>` dropdown for switching languages
  - A `Job` named `config-applier` that:
    - Waits for `index.html` to be written
    - Applies configuration values (default language & background color) from the `ConfigMap`

- `argocd-webquote.yaml`:  
  Defines the [ArgoCD Application](https://argo-cd.readthedocs.io/en/stable/operator-manual/application/) that syncs this app to your Kubernetes cluster.  
  Features:
  - Auto-sync enabled
  - Namespace auto-creation
  - Sync retry with exponential backoff
  - Rollback on sync failure

## 🚀 Deployment with ArgoCD

1. **Install ArgoCD** (if not already installed)  
   See: [https://argo-cd.readthedocs.io/en/stable/getting_started/](https://argo-cd.readthedocs.io/en/stable/getting_started/)

2. **Login to ArgoCD CLI or Web UI**
   ![image](https://github.com/user-attachments/assets/46b40deb-18e2-4c6d-b0dd-a8d031dab5e6)


4. **Create the Application Stack**

```
# On host
## Check contexts

$ kubectl config get-contexts
CURRENT   NAME                 CLUSTER              AUTHINFO             NAMESPACE
          kind-cka-cluster1    kind-cka-cluster1    kind-cka-cluster1
          kind-cka-cluster12   kind-cka-cluster12   kind-cka-cluster12
          kind-cka-cluster2    kind-cka-cluster2    kind-cka-cluster2
*         kind-kind            kind-kind            kind-kind            datapipeline
          kind-kindcluster     kind-kindcluster     kind-kindcluster

## Create namespace

$ kubectl create namespace argocd
namespace/argocd created

$ kubectl get namespaces
NAME                 STATUS   AGE
argocd               Active   10h
default              Active   26d
instavote            Active   26d
kube-node-lease      Active   26d
kube-public          Active   26d
kube-system          Active   26d
local-path-storage   Active   26d
trino                Active   8d

## Install argocd 

$ kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

## Check argocd

$ kubectl get pod,svc -n argocd
NAME                                                    READY   STATUS              RESTARTS   AGE
pod/argocd-application-controller-0                     0/1     ContainerCreating   0          82s
pod/argocd-applicationset-controller-74cf9dbb5f-splvn   0/1     ContainerCreating   0          83s
pod/argocd-dex-server-54dd57bdc5-t9sgw                  0/1     Init:0/1            0          83s
pod/argocd-notifications-controller-65cf4b7cfd-tkfjt    0/1     ContainerCreating   0          83s
pod/argocd-redis-b6fc9657b-57d77                        0/1     Init:0/1            0          83s
pod/argocd-repo-server-dc895cd45-5fbfn                  0/1     Init:0/1            0          83s
pod/argocd-server-6576896669-7727m                      0/1     ContainerCreating   0          82s

NAME                                              TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
service/argocd-applicationset-controller          ClusterIP   10.96.30.80     <none>        7000/TCP,8080/TCP            86s
service/argocd-dex-server                         ClusterIP   10.96.60.124    <none>        5556/TCP,5557/TCP,5558/TCP   86s
service/argocd-metrics                            ClusterIP   10.96.88.163    <none>        8082/TCP                     86s
service/argocd-notifications-controller-metrics   ClusterIP   10.96.115.4     <none>        9001/TCP                     86s
service/argocd-redis                              ClusterIP   10.96.50.169    <none>        6379/TCP                     86s
service/argocd-repo-server                        ClusterIP   10.96.167.196   <none>        8081/TCP,8084/TCP            85s
service/argocd-server                             ClusterIP   10.96.38.0      <none>        80/TCP,443/TCP               85s
service/argocd-server-metrics                     ClusterIP   10.96.233.179   <none>        8083/TCP                     85s

## Port forward

$ kubectl port-forward svc/argocd-server -n argocd 8080:443
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
Handling connection for 8080

## UI password

$ kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath='{.data.password}' | base64 -d

## Create git repo

git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:sanjeevtripurari/argocd-web.git
git push -u origin main

## Adding project to git repo

$ git status
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        webquote/

$ git add -A

$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   webquote/nginx-deployment.yaml


$ git commit -m "commit nginx-deployment" -a
[main eeb84d2] commit nginx-deployment
 1 file changed, 33 insertions(+)
 create mode 100644 webquote/nginx-deployment.yaml


$ git push -u origin main
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 8 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (5/5), 640 bytes | 160.00 KiB/s, done.
Total 5 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To github.com:sanjeevtripurari/argocd-web.git
   ef37d91..eeb84d2  main -> main
branch 'main' set up to track 'origin/main'.

## auto creation of namespace: webquote

$ kubectl get namespaces
NAME                 STATUS   AGE
argocd               Active   10h
default              Active   26d
instavote            Active   26d
kube-node-lease      Active   26d
kube-public          Active   26d
kube-system          Active   26d
local-path-storage   Active   26d
trino                Active   8d
webquote             Active   9h

$ kubectl get pod,svc -n webquote
NAME                       READY   STATUS      RESTARTS   AGE
pod/config-applier-lszj8   0/1     Completed   0          42m
pod/content-writer-47v4q   0/1     Completed   0          42m
pod/nginx-web              1/1     Running     0          9h

NAME                    TYPE       CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
service/nginx-service   NodePort   10.96.44.87   <none>        80:30081/TCP   9h

```
