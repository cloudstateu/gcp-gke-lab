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

1. Open terminal and lunch command: 
* <code>kubectl create namespace app1</code>
* <code>kubectl create namespace app2</code>
2. Set default namespace:
* <code>kubectl config set-context --current --namespace=app1ns </code>
3. Validate it
* <code>kubectl config view | grep namespace:</code>


## Task 2: Check resources in namespaces and out of namespaces
1.	Run in terminal command:
* View all resources in namespaces: <code>kubectl api-resources --namespaced=true</code>
* View all resources out of namespaces: <code>kubectl api-resources --namespaced=false</code>


## Task 3: Deploy application to Web App.
1. Build application using command: <code>ng build</code>
2. Click on Azure icon in left menu in VS Code.
3. Click Singin to Azure.
4. Select your Web Application.
5. Right-click on the web app in the Azure App Service extension and select the “Deploy to Web App” option.
6. Set source directory to <code>/dist/fe-visual</code>
7. Select Yes on the “Are you sure you want to deploy…” dialog to overwrite any previous deployments you may have done to your Azure Web App.
8. Open your web app URL and check if page is working.

## Task 4: Create network policy
1. Create app1policy.yml file.
2. Copy network policy definition: 
`
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: app1-policy
  namespace: app1ns
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - namespaceSelector: {}
  egress:
  - to:
    - namespaceSelector: {}
`


4. Open file metrics.ts and insert a code: 
* <code> 
    export interface Metrics { 
    public a1: number;
    public b1: number;
    public Time: Date;
}
</code>
5. Create a data model class using command:
* <code>ng g i data</code>
6. Open file data.ts and insert a code:
* <code>
import { Metrics } from './metrics';
export interface Data {
     msgType: string;
     data?: Metrics;
     error?: any;
}
</code>
7. Install ngx-socket-io using code: <code>npm i ngx-socket-io --save</code>
8. Copy code from file */lab3-files/app.module.ts* to app.module.ts file.
9. Copy code from file */lab3-files/app.component.ts* to app.component.ts.
9. Copy code from file */lab3-files/app.component.html* to app.component.html.
9. Copy code from file */lab3-files/polyfills.ts* to polyfills.ts.
10. Build solution.

## Task 5: Add deployment slots
1.	On the Azure Portal, go to the previously created instance of Azure Web App and click on Deployment slots. 
2.	On the Deployment slot page click on the Add a slot button marked with a plus sign.
3.	On the Add a slot blade provide below configuration:
* Name: stage
* Configuration Source: Don’t clone configuration from an exisiting slot.
4. Go to VS Code and deploy new solution to deployment stage deployment slot. 
5. On the Azure Portal go to the Deployment slots and click on created stage slot.
5. On the stage slot of Azure Web App copy URL link to any web browser.
14.	Then you should see updated stage ASP.NET MVC application.
15.	On the Overview page of non-stage production Web App click on Swap.
16.	On the Swap blade provide the following configurations:
•	Swap type: Swap
•	Source: stage
•	Destination: production

And click OK.

17.	After swapping Web App slots, copy URL link to any web browser.


Next click on OK.

## Task 6: Authorize using AAD
1.	On the Azure Portal, go to the previously created instance of Azure Web App and click on Authentication / Authorization.
2.	On Authentication / Authorizaton page provide below configuration:
* App Service Authentication: On.
* Action to take when request is not authenticated: Log in with Azure Active Directory.
3.	On Authentication Providers section click on Azure Active Directory, then you will be moved in new page Azure Active Directory Settings.
4.	On Azure Active Directory Setting page provide below configuration:
* Management mode: Express
* Management mode: web-app-student0x
* Grant Common Data Services Permissions: Off
And click on OK.
5. Finally click on Save to saving authorization settings.
6. Open the web browser in private mode and copy again URL link of Azure Web App.
7. We will be asked to authenticate using credentials which was used to logging to Azure Portal.
8. After correct authentication, you should see the running ASP.NET MVC application on Azure Web App.

<br><br>

<center><p>&copy; 2019 Chmurowisko Sp. z o.o.<p></center>
