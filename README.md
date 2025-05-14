
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

UI password
$ kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath='{.data.password}' | base64 -d

# Create git repo

git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:sanjeevtripurari/argocd-web.git
git push -u origin main


Addin ninx

$ git status
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        webquote/

nothing added to commit but untracked files present (use "git add" to track)

Madhavi@madhavikrishna /cygdrive/d/Sanjeev/docker/kubernetes/argocd-web
$ git add -a
error: unknown switch `a'
usage: git add [<options>] [--] <pathspec>...

    -n, --[no-]dry-run    dry run
    -v, --[no-]verbose    be verbose

    -i, --[no-]interactive
                          interactive picking
    -p, --[no-]patch      select hunks interactively
    -e, --[no-]edit       edit current diff and apply
    -f, --[no-]force      allow adding otherwise ignored files
    -u, --[no-]update     update tracked files
    --[no-]renormalize    renormalize EOL of tracked files (implies -u)
    -N, --[no-]intent-to-add
                          record only the fact that the path will be added later
    -A, --[no-]all        add changes from all tracked and untracked files
    --[no-]ignore-removal ignore paths removed in the working tree (same as --no-all)
    --[no-]refresh        don't add, only refresh the index
    --[no-]ignore-errors  just skip files which cannot be added because of errors
    --[no-]ignore-missing check if - even missing - files are ignored in dry run
    --[no-]sparse         allow updating entries outside of the sparse-checkout cone
    --[no-]chmod (+|-)x   override the executable bit of the listed files
    --[no-]pathspec-from-file <file>
                          read pathspec from file
    --[no-]pathspec-file-nul
                          with --pathspec-from-file, pathspec elements are separated with NUL character


Madhavi@madhavikrishna /cygdrive/d/Sanjeev/docker/kubernetes/argocd-web
$ git add -A

Madhavi@madhavikrishna /cygdrive/d/Sanjeev/docker/kubernetes/argocd-web
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   webquote/k8s/nginx-deployment.yaml


Madhavi@madhavikrishna /cygdrive/d/Sanjeev/docker/kubernetes/argocd-web
$ git commit -m "commit nginx-deployment" -a
[main eeb84d2] commit nginx-deployment
 1 file changed, 33 insertions(+)
 create mode 100644 webquote/k8s/nginx-deployment.yaml

Madhavi@madhavikrishna /cygdrive/d/Sanjeev/docker/kubernetes/argocd-web
$

Madhavi@madhavikrishna /cygdrive/d/Sanjeev/docker/kubernetes/argocd-web
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

```
