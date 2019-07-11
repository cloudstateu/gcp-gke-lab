<img src="https://avatars1.githubusercontent.com/u/47143554?s=400&u=7c55eeec6479b4ff59df7cad452501a41635b0e4&v=4" alt="Cloudstate logo" width="200" align="right">
<br><br>
<br><br>
<br><br>

# Lab3: Multi tenancy cluster

## LAB Overview
We will create multitenancy architecture for two tenants with full isolation of resources, traffic and compute power.

#### This lab will demonstrate:
* Creating a namespaces
* Network policy.


## Task 1: Create new namespace
1. Create deployment file for namespace:
'''
apiVersion: v1
kind: Namespace
metadata:
   name: app1-dl-ns
   labels:
     project: app1
'''
2. Open terminal and lunch command: 
* <code>kubectl apply -f [PATH_TO_DEPLOYMENT_FILE]</code>
3. Set default namespace:
* <code>kubectl config set-context --current --namespace=app1-dl-ns </code>
4. Validate it
* <code>kubectl config view | grep namespace:</code>
5. Create namespace app1-be-ns doing point 1 and 2.


## Task 2: Check resources in namespaces and out of namespaces
1.	Run in terminal command:
* View all resources in namespaces: <code>kubectl api-resources --namespaced=true</code>
* View all resources out of namespaces: <code>kubectl api-resources --namespaced=false</code>


## Task 3: Deploy MongoDB to namespace app1ns.
1. Deploy mongo DB using yml:
```
apiVersion: apps/v1beta1 
kind: Deployment 
metadata: 
  name: mongo
  namespace: app1-dl-ns
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
  namespace: app1-dl-ns
spec: 
  type: ClusterIP 
  ports: 
  - port: 27017 
  selector: 
    app: mongo 
```
2. Check pod and service status.
3. Create berealtime container in second namespace using yaml:
```
apiVersion: apps/v1beta1 
kind: Deployment 
metadata: 
 name: berealtime 
 namespace: app1-be-ns
spec: 
 replicas: 1 
 template: 
   metadata: 
     labels: 
       app: berealtime 
   spec: 
     containers: 
     - name: berealtime 
       image: gcr.io/chmurowiskolab/berealtime 
       ports: 
       - containerPort: 3000 
       env:
         - name: DBURL
           value: mongodb://admin:secret@mongo.app1-dl-ns:27017
         - name: TIMEOUT
           value: '5000'
     imagePullSecrets: 
     - name: SECRET_NAME
---
apiVersion: v1 
kind: Service 
metadata: 
 name: berealtime 
 namespace: app1-be-ns
spec: 
 type: LoadBalancer
 ports: 
 - port: 80 
   targetPort: 3000
 selector: 
   app: berealtime 
```
3. Click Singin to Azure.
4. Select your Web Application.
5. Right-click on the web app in the Azure App Service extension and select the “Deploy to Web App” option.
6. Set source directory to <code>/dist/fe-visual</code>
7. Select Yes on the “Are you sure you want to deploy…” dialog to overwrite any previous deployments you may have done to your Azure Web App.
8. Open your web app URL and check if page is working.

## Task 4: Create network policy
1. Create app1policy.yml file.
2. Copy network policy definition: 
```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: app1-policy
  namespace: app1-dl-ns
spec:
  podSelector: {}       
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          project: app1
      podSelector: {}
  egress:
  - to:
    - namespaceSelector:
        matchLabels: 
          project: app1
      podSelector: {}
```
## Task 5: Specify nodes for pods.
1. Label nodes using command:
<code>kubectl label nodes <node-name> project=app1-az1</code>
2. Update deployment files and add:

```
affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: project
            operator: In
            values:
            - app1-az1
            - app1-az2
 ```
So it will looks like this:
```
apiVersion: apps/v1beta1 
kind: Deployment 
metadata: 
 name: berealtime 
 namespace: app1-be-ns
spec: 
 replicas: 1 
 template: 
   metadata: 
     labels: 
       app: berealtime 
   spec: 
    affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: project
            operator: In
            values:
            - app1-az1
            - app1-az2
    containers: 
    - name: berealtime 
      image: gcr.io/chmurowiskolab/berealtime 
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
 namespace: app1-be-ns
spec: 
 type: LoadBalancer
 ports: 
 - port: 80 
   targetPort: 3000
 selector: 
   app: berealtime 
```
<center><p>&copy; 2019 Chmurowisko Sp. z o.o.<p></center>
