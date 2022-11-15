# Service

You have deployed an application, that is listening at port 8000.
You used a replica-set to deploy it and created a NodePort service to make it accessible.
But when you test it, somehow the application is not reachable at the port.
Write down your approach and sequentially all the steps that you will take to find out the issue.


Port 8000 needs to be exposed to the host.
In K8s this can be achieved by using nodePort, as demostrated in 05-services/kubia-svc-nodeport.yaml changeing the key "port" from 80 to 8000:

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
