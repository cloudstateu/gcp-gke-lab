<img src="https://avatars1.githubusercontent.com/u/47143554?s=400&u=7c55eeec6479b4ff59df7cad452501a41635b0e4&v=4" alt="Cloudstate logo" width="200" align="right">
<br><br>
<br><br>
<br><br>

# Lab4: Persistant Volumes.

## LAB Overview


## Task 1: Create PV
1. Restart mongo db POD.
2. Create deployment file:
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mongo-disk
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 30Gi
```
3. Run deployment file in terminal.


## Task 2: MOdify deployment file for db

```
apiVersion: apps/v1beta1 
kind: Deployment 
metadata: 
  name: mongo
  namespace: app1ns
spec: 
  replicas: 1 
  template: 
    metadata: 
      labels: 
        app: mongo 
    spec: 
      nodeName: gke-standard-cluster-1-default-pool-08cc427a-cdz6
      containers: 
      - name: mongo 
        image: mongo 
        volumeMounts:
        - mountPath: "/data/db"
          name: volume
        ports: 
        - containerPort: 27017 
        env:
          - name: MONGO_INITDB_ROOT_USERNAME
            value: admin
          - name: MONGO_INITDB_ROOT_PASSWORD
            value: secret
        volumes:
        - name: volume
          persistentVolumeClaim:
            claimName: mongo-disk
---
apiVersion: v1 
kind: Service 
metadata: 
  name: mongo 
  namespace: app1ns
spec: 
  type: ClusterIP 
  ports: 
  - port: 27017 
  selector: 
    app: mongo 
 ```
2. Apply file in terminal.

## Task 3: Check if it works
1. Restart mongo container.

<br><br>

<center><p>&copy; 2019 Chmurowisko Sp. z o.o.<p></center>
