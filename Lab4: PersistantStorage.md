<img src="https://avatars1.githubusercontent.com/u/47143554?s=400&u=7c55eeec6479b4ff59df7cad452501a41635b0e4&v=4" alt="Cloudstate logo" width="200" align="right">
<br><br>
<br><br>
<br><br>

# Lab4: Persistant Volumes.

## LAB Overview


## Task 1: Create PV

1. Create deployment file:
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
2. Run deployment file in terminal.


## Task 2: Create build definition
1. Click Pipelines -> Builds on left menu.
2. Click *New pipeline* button.
3. Choose Azure repository and select your repo.
4. Select Nodejs with Angular as pipeline configuration.
5. Add task: *Publish artifact* and choose target path *dist*
5. Click *Save and run*

## Task 3: Create realese definition
1. Click Pipelines -> Releases on left menu.
2. Click *New pipeline* button.
3. Select template *Azure App Service deployment*.
4. Click *Add an artifact*.
5. Select artifact from build.
6. Click on *1 job* at stage1.
7. Configure Azure Subsrciption and click *Authorize*
8. Choose a web app created at lab 3.
9. Click on *Deploy Azure App Service*
10. Choose resource group from dropbox.
11. Select stage deployment slot.
12. Click *Save*
13. Go to Pipelines -> Releases
14. Click *Create a release*
15. Choose a stage env and click *Start*.

<br><br>

<center><p>&copy; 2019 Chmurowisko Sp. z o.o.<p></center>
