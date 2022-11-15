# Service

You have deployed an application, that is listening at port 8000.
You used a replica-set to deploy it and created a NodePort service to make it accessible.
But when you test it, somehow the application is not reachable at the port.
Write down your approach and sequentially all the steps that you will take to find out the issue.


Port 8000 needs to be exposed to the host, in K8s this can be achieved by using NodePort as stated and demostrated in 05-services/kubia-svc-nodeport.yaml changeing the key "port" from 80 to 8000:

```yaml
piVersion: v1
kind: Service
metadata:
  name: kubia-nodeport
spec:
  type: NodePort
  ports:
  - port: 8000
    targetPort: 8080
    nodePort: 30123
  selector:
    app: kubia
```

NodePort exposes application to host - therefore localhost:nodeport socket will not work, but publicip:nodeport socket will.
```sh

[root@ip-172-31-4-106 05-services]# kx get po -o wide
NAME          READY   STATUS    RESTARTS   AGE     IP                NODE                                               NOMINATED NODE   READINESS GATES
kubia-vlhzf   1/1     Running   0          3m26s   192.168.137.154   ip-172-31-11-169.ap-southeast-1.compute.internal   <none>           <none>
kubia-wths4   1/1     Running   0          3m26s   192.168.137.137   ip-172-31-11-169.ap-southeast-1.compute.internal   <none>           <none>
kubia-z5lxx   1/1     Running   0          3m26s   192.168.137.145   ip-172-31-11-169.ap-southeast-1.compute.internal   <none>           <none>
[root@ip-172-31-4-106 05-services]#

# Locally exposed port (8080) works with pod's IP:
[root@ip-172-31-4-106 05-services]# curl 192.168.137.154:8080
You've hit kubia-vlhzf

# NodePort does not work with pod's IP:
[root@ip-172-31-4-106 05-services]# curl 192.168.137.154:30123
curl: (7) Failed to connect to 192.168.137.154 port 30123 after 0 ms: Connection refused

# NodePort works with host's IP:
[root@ip-172-31-4-106 05-services]# curl 172.31.4.106:30123
You've hit kubia-z5lxx
```
