<img src="https://avatars1.githubusercontent.com/u/47143554?s=400&u=7c55eeec6479b4ff59df7cad452501a41635b0e4&v=4" alt="Cloudstate logo" width="200" align="right">
<br><br>
<br><br>
<br><br>

# Lab4: Ingres controler.

## Task 1: Prepare
1. Create new nginx deployment.
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
---
apiVersion: v1 
kind: Service 
metadata: 
  name: nginx 
  namespace: app1-be-ns
spec: 
  type: ClusterIp 
  ports: 
  - port: 80 
  selector: 
    app: nginx 
```

## Task 2: Create ingress:
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: app1ing
spec:
  rules:
  - http:
      paths:
      - path: /*
        backend:
          serviceName: nginx
          servicePort: 80
      - path: /api/*
        backend:
          serviceName: berealtime
          servicePort: 80
```
2. Apply deployment files.

<br><br>

<center><p>&copy; 2019 Chmurowisko Sp. z o.o.<p></center>
