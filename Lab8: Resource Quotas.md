

<img src="https://avatars1.githubusercontent.com/u/47143554?s=400&u=7c55eeec6479b4ff59df7cad452501a41635b0e4&v=4" alt="Cloudstate logo" width="200" align="right">
<br><br>
<br><br>
<br><br>

# Lab8: Resource Quotas.

## Task1: Create deployment with simple resource quotas.

1.	Create file deployment.yml:

```
apiVersion: apps/v1 
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: app1-be-ns
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 1
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
        resources:
          requests:
            cpu: "1"
            memory: "64Mi"
          limits:
            memory: "128Mi"
            cpu: "1500m"
---
apiVersion: v1 
kind: Service 
metadata: 
  name: nginx 
  namespace: app1-be-ns
spec: 
  type: LoadBalancer 
  ports: 
  - port: 80 
  selector: 
    app: nginx
```
2. Lets increase number of replicas to 7 and check what happen.

## Task2: Create custom quotas on nodes:
1. Open tunnel to K8s klaster using command:
<code>kubectl proxy</code>
2. Open new terminal, provide master ip and node name and run command:

```
curl --header "Content-Type: application/json-patch+json" \
--request PATCH \
--data '[{"op": "add", "path": "/status/capacity/app1pods", "value": "3"}]' \
http://[private-address-to-master-server]/api/v1/nodes/[node-id]/status
```
3. Modify deployment file and add resource quotas:
```
apiVersion: apps/v1 
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: app1-be-ns
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 1
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
        resources:
          requests:
            app1pods: "1"
          limits:
            app1pods: "1"
---
apiVersion: v1 
kind: Service 
metadata: 
  name: nginx 
  namespace: app1-be-ns
spec: 
  type: LoadBalancer 
  ports: 
  - port: 80 
  selector: 
    app: nginx
```
4. Increase pods replicas number to 10.

<center><p>&copy; 2019 Chmurowisko Sp. z o.o.<p></center>


