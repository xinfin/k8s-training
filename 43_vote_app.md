# Vote app demo

```sh
[root@ip-172-31-6-142 ~]# cd example-voting-app/
[root@ip-172-31-6-142 example-voting-app]# l
total 96
-rw-r--r-- 1 root root 54824 Nov 15 10:37 architecture.png
-rw-r--r-- 1 root root   609 Nov 15 10:37 dockercloud.yml
-rw-r--r-- 1 root root   808 Nov 15 10:37 docker-compose-javaworker.yml
-rw-r--r-- 1 root root   400 Nov 15 10:37 docker-compose-simple.yml
-rw-r--r-- 1 root root   808 Nov 15 10:37 docker-compose.yml
-rw-r--r-- 1 root root  1692 Nov 15 10:37 docker-stack.yml
drwxr-xr-x 2 root root   250 Nov 15 10:37 k8s-specifications
-rw-r--r-- 1 root root 10758 Nov 15 10:37 LICENSE
-rw-r--r-- 1 root root   185 Nov 15 10:37 MAINTAINERS
-rw-r--r-- 1 root root  2182 Nov 15 10:37 README.md
drwxr-xr-x 5 root root   133 Nov 15 10:37 result
drwxr-xr-x 4 root root    93 Nov 15 10:37 vote
drwxr-xr-x 3 root root    70 Nov 15 10:37 worker
[root@ip-172-31-6-142 example-voting-app]# kx apply -f k8s-specifications/
deployment.apps/db created
service/db created
deployment.apps/redis created
service/redis created
deployment.apps/result created
service/result created
deployment.apps/vote created
service/vote created
deployment.apps/worker created
[root@ip-172-31-6-142 example-voting-app]# kx get po -o wide
NAME                      READY   STATUS    RESTARTS   AGE   IP               NODE                                               NOMINATED NODE   READINESS GATES
db-b54cd94f4-vm5tf        1/1     Running   0          31s   192.168.80.150   ip-172-31-12-255.ap-southeast-1.compute.internal   <none>           <none>
redis-868d64d78-5swpf     1/1     Running   0          31s   192.168.80.151   ip-172-31-12-255.ap-southeast-1.compute.internal   <none>           <none>
result-5d57b59f4b-d2bzf   1/1     Running   0          31s   192.168.80.153   ip-172-31-12-255.ap-southeast-1.compute.internal   <none>           <none>
vote-94849dc97-h5m66      1/1     Running   0          31s   192.168.80.152   ip-172-31-12-255.ap-southeast-1.compute.internal   <none>           <none>
worker-dd46d7584-smc9m    1/1     Running   0          31s   192.168.80.154   ip-172-31-12-255.ap-southeast-1.compute.internal   <none>           <none>
[root@ip-172-31-6-142 example-voting-app]# alias kx
alias kx='kubectl --kubeconfig=/root/.kube/xconf'
[root@ip-172-31-6-142 example-voting-app]# cat ~/.kube/xconf | grep namespace
    namespace: xinfin
[root@ip-172-31-6-142 example-voting-app]#
```
