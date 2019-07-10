<img src="https://avatars1.githubusercontent.com/u/47143554?s=400&u=7c55eeec6479b4ff59df7cad452501a41635b0e4&v=4" alt="Cloudstate logo" width="200" align="right">
<br><br>
<br><br>
<br><br>

# Create simple deployment on GKE

## LAB Overview

#### This lab will demonstrate:
* How to create namespace.
* How to deploy POD and service.
* How to create healtcheck


## Task 2: Create secret and deploy POD

In this section, you will learn how to create deployment file and how to deploy it. 
 

1. Create deployment file (deployment-db.yml): 
```
apiVersion: apps/v1beta1 
kind: Deployment 
metadata: 
  name: mongo 
spec: 
  replicas: 1 
  template: 
    metadata: 
      labels: 
        app: mongo 
    spec: 
      containers: 
      - name: mongo 
        image: mongo 
        ports: 
        - containerPort: 27017 
        env:
          - name: MONGO_INITDB_ROOT_USERNAME
            value: admin
          - name: MONGO_INITDB_ROOT_PASSWORD
            value: secret
---
apiVersion: v1 
kind: Service 
metadata: 
  name: mongo 
spec: 
  type: ClusterIP 
  ports: 
  - port: 3000 
  selector: 
    app: mongo 
```
 3. Open console and type: <code>kubectl create –f pathToDeploymentFile </code>
 4. Verify status using command: <code>kubectl get pods</code>
 5. Create application deployment file (deployment-app.yml):
 ```
apiVersion: apps/v1beta1 
kind: Deployment 
metadata: 
  name: berealtime 
spec: 
  replicas: 1 
  template: 
    metadata: 
      labels: 
        app: berealtime 
    spec: 
      containers: 
      - name: berealtime 
        image: [host]/[project-id]/berealtime 
        ports: 
        - containerPort: 3000 
        env:
          - name: DBURL
            value: mongodb://admin:secret@mongo:27017
          - name: TIMEOUT
            value: '5000'
      imagePullSecrets: 
      - name: SECRET_NAME
---
apiVersion: v1 
kind: Service 
metadata: 
  name: berealtime 
spec: 
  type: LoadBalancer
  ports: 
  - port: 3000 
  selector: 
    app: berealtime 
```
 6. Open console and type: <code>kubectl create –f pathToDeploymentFile </code>
 7. Verify status using command: <code>kubectl get pods</code>
 8. Type command kubectl get services and open service address in web browser.
