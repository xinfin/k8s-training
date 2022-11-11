# Deployment

Suppose you have deployed your application using a deployment controller. Assume the initial number of replicas is one.
Write the steps needed to update a container's image using deployment, making sure that there is zero downtime.

To perform rolling update to v2 of "kubia" do:
```sh
kubectl apply -f v1.yaml
```
Afterwards we can show the status of provisioned pods by below:
```sh
kubectl get po
```
Shows 3 pods kubia.

Next we apply v2:
```sh
kubectl apply -f v2.yaml
```
2 pods are terminated, 1 of the old pods is kept, while 1 new (with container created from the new version of image [v2]) is provisioned,
afterwhich the old pod is terminated.

```sh
kubectl get po
```
After the provisioning is done, the above shows 1 pod kubia (v2)

Next we apply v3:
```sh
kubectl apply -f v3.yaml
```
The old pod is kept, while 3 new (with container created from the new version of image [v3]) is provisioned, afterwhich the old pod is terminated.

```sh
kubectl get po
```
After provisioning is done, above shows 3 pods kubia (v3)
Where the type "Deployment" makes sure that the defined number of replicas will be kept running (the old version) until the new version is provisioned and up.


Configuration (yaml) files used in above steps:
- v1.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: kubia
spec:
  selector:
    matchLabels:
      app: kubia
  replicas: 3
  template:
    metadata:
      name: kubia
      labels:
        app: kubia
    spec:
      containers:
      - image: luksa/kubia:v1
        name: nodejs

# source kubernetes-training/09-deployments/kubia-deployment-v1.yaml 
```

- v2.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: kubia
spec:
  selector:
    matchLabels:
      app: kubia
  template:
    metadata:
      name: kubia
      labels:
        app: kubia
    spec:
      containers:
      - image: luksa/kubia:v2
        name: nodejs

# source kubernetes-training/09-deployments/kubia-deployment-v1.yaml 
```

- v3.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: kubia
spec:
  selector:
    matchLabels:
      app: kubia
  replicas: 3
  template:
    metadata:
      name: kubia
      labels:
        app: kubia
    spec:
      containers:
      - image: luksa/kubia:v3
        name: nodejs

# source kubernetes-training/09-deployments/kubia-deployment-v1.yaml 
```
