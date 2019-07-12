<img src="https://avatars1.githubusercontent.com/u/47143554?s=400&u=7c55eeec6479b4ff59df7cad452501a41635b0e4&v=4" alt="Cloudstate logo" width="200" align="right">
<br><br>
<br><br>
<br><br>

# Lab4: Istio.

## Task1: Download istioctl and deploy istio on k8s cluster.
1. Open page https://github.com/istio/istio/releases and download realease on your os.
2. Extract the downloaded installation file. The installation directory contains:
* Installation .yaml files for Kubernetes in install/
* Sample applications in samples/
* The istioctl client binary in the bin/ directory. istioctl is used when manually injecting Envoy as a sidecar proxy and for creating routing rules and policies.
* The istio.VERSION configuration file
3. Add the istioctl client to your PATH:
<code>export PATH=$PWD/bin:$PATH</code>
4. Grant cluster admin permissions to the current user. You need these permissions to create the necessary role based access control (RBAC) rules for Istio:
```
kubectl create clusterrolebinding cluster-admin-binding \
  --clusterrole=cluster-admin \
  --user="$(gcloud config get-value core/account)"
```
5.Install Istio's core components:

<code>kubectl apply -f install/kubernetes/istio-demo-auth.yaml</code>

6. Veryfi services and pods in istio-system namespace.


## Task3: Deploy sample app
1. Deploy the application using kubectl apply and istioctl kube-inject. The kube-inject command updates the BookInfo deployment so that a sidecar is deployed in each application pod along with the service.
2. Run command:
<code> kubectl apply -f <(istioctl kube-inject -f samples/helloworld/helloworld.yaml)</code>
3. Add ingres:
    <code>kubectl apply -f samples/helloworld/helloworld-gateway.yaml</code>

## Task4: Validate deployment:
1. Find IP adress of ingress:
<code>kubectl get svc istio-ingressgateway -n istio-system</code>
2. Run in browser ip from ingress.

<br><br>

<center><p>&copy; 2019 Chmurowisko Sp. z o.o.<p></center>


