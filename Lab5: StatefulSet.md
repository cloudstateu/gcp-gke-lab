<img src="https://avatars1.githubusercontent.com/u/47143554?s=400&u=7c55eeec6479b4ff59df7cad452501a41635b0e4&v=4" alt="Cloudstate logo" width="200" align="right">
<br><br>
<br><br>
<br><br>

# Lab4: Scale stateful application.

## Task 1: Prepare
1. Scale up numbers of pods for mongo
2. Delete deployment and service.

## Task 2: Create statefulset:
1. Create deployment file:
```
---
apiVersion: v1 
kind: Service 
metadata: 
  name: mongo 
  namespace: app1-dl-ns
spec: 
  type: ClusterIP 
  ports: 
  - port: 27017 
  selector: 
    app: mongo 
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mongo
  namespace: app1-dl-ns
spec:
  serviceName: "mongo"
  replicas: 2
  selector:
    matchLabels:
      app: mongo
  template:
    metadata:
      labels:
        app: mongo
    spec:
      containers:
      - name: mongo
        image: mongo
        ports:
        - containerPort: 80
          name: web
        volumeMounts:
        - name: db
          mountPath: /data/db
  volumeClaimTemplates:
  - metadata:
      name: db
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 1Gi
```
2. Run database.

<br><br>

<center><p>&copy; 2019 Chmurowisko Sp. z o.o.<p></center>
