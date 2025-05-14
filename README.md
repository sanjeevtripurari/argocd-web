
# argocd-web

```
Ceck contexts

$ kubectl config get-contexts
CURRENT   NAME                 CLUSTER              AUTHINFO             NAMESPACE
          kind-cka-cluster1    kind-cka-cluster1    kind-cka-cluster1
          kind-cka-cluster12   kind-cka-cluster12   kind-cka-cluster12
          kind-cka-cluster2    kind-cka-cluster2    kind-cka-cluster2
*         kind-kind            kind-kind            kind-kind            datapipeline
          kind-kindcluster     kind-kindcluster     kind-kindcluster

Create namespace

$ kubectl create namespace argocd
namespace/argocd created

Install argocd 

$ kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml


Ceck argocd

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

Port forward

$ kubectl port-forward svc/argocd-server -n argocd 8080:443
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
Handling connection for 8080

```
