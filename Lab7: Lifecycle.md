
<img src="https://avatars1.githubusercontent.com/u/47143554?s=400&u=7c55eeec6479b4ff59df7cad452501a41635b0e4&v=4" alt="Cloudstate logo" width="200" align="right">
<br><br>
<br><br>
<br><br>

# Lab4: Pod Lifecycle.

## Task1: Download istioctl and deploy istio on k8s cluster.
 In this exercise, you create a Pod that runs a Container based on the k8s.gcr.io/busybox image and add probes to it.
1.	Create file deployment.yml:

```
apiVersion: v1
kind: Pod
metadata:
  labels:
    test: liveness
  name: liveness-exec
spec:
  containers:
  - name: liveness
    image: k8s.gcr.io/busybox
    args:
    - /bin/sh
    - -c
    - touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600
    livenessProbe:
      exec:
        command:
        - cat
        - /tmp/healthy
      initialDelaySeconds: 5
      periodSeconds: 5
```
2.	In the configuration file, you can see that the Pod has a single Container. The periodSeconds field specifies that the kubelet should perform a liveness probe every 5 seconds. When the Container starts, it executes this command: 
<code>/bin/sh -c "touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600"</code>
3.	Create pod by command: <code> kubectl create -f {path-to-your-file} </code>
4.	Within first 30 seconds check pod status using command: kubectl describe pod liveness-exec
5.	Wait next 30 seconds and check again: kubectl describe pod liveness-exec 


## Task3: Define a liveness HTTP request
1.	Create file deployment.yml:
```
apiVersion: v1
kind: Pod
metadata:
  labels:
    test: liveness
  name: liveness-http
spec:
  containers:
  - name: liveness
    image: k8s.gcr.io/liveness
    args:
    - /server
    livenessProbe:
      httpGet:
        path: /healthz
        port: 8080
        httpHeaders:
        - name: Custom-Header
          value: Awesome
      initialDelaySeconds: 3
      periodSeconds: 3
```
For the first 10 seconds that the Container is alive, the /healthz handler returns a status of 200. After that, the handler returns a status of 500.
2.	Create pod by command: <code>kubectl create -f {path-to-your-file}</code>
3.	After 10 seconds check pod status using command: <code>kubectl describe pod liveness-http</code>

<center><p>&copy; 2019 Chmurowisko Sp. z o.o.<p></center>


