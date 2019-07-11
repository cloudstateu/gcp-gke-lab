
<img src="https://avatars1.githubusercontent.com/u/47143554?s=400&u=7c55eeec6479b4ff59df7cad452501a41635b0e4&v=4" alt="Cloudstate logo" width="200" align="right">
<br><br>
<br><br>
<br><br>

# Lab3b: Secrets.

## LAB Overview


## Task 1: Create secrets for mongodb
1. Create usernam and password by command:
<code>kubectl create secret generic prod-db-secret --from-literal=username=admin --from-literal=password=secret</comand>
2. Modify deployment file of db to set environment
```
        env:
          - name: MONGO_INITDB_ROOT_USERNAME
            valueFrom:
                  secretKeyRef:
                        name: prod-db-secret
                        key: username
          - name: MONGO_INITDB_ROOT_PASSWORD
            valueFrom:
                  secretKeyRef:
                        name: prod-db-secret
                        key: password                        
```

<br><br>
3. Do the same for berealtime pod.

<center><p>&copy; 2019 Chmurowisko Sp. z o.o.<p></center>


