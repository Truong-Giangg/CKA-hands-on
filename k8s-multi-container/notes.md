##Init container run first, main container wait for init container
```
k describe pod myapp-pod
```
Check pod logs:
```
k logs myapp-pod
```
Check container logs (logs of command run inside container) inside pod
```
k logs myapp-pod -c init-myservice
```
##Create pods & service to make init container done running
Create pods by using deployment
```
k create deployment nginx-deploy --image=nginx --port 80
```
Create service
```
k expose deployment --name=myservice nginx-deploy --port 80
```
After create the service, nslookup pass so main container should run
```
$ k get pod
NAME                            READY   STATUS    RESTARTS   AGE
myapp-pod                       1/1     Running   0          11m
nginx-deploy-5c477b75b8-thtx5   1/1     Running   0          4m14s
```
Init container logs should return:
```
k logs myapp-pod -c init-myservice
Name:      myservice.default.svc.cluster.local
Address 1: 10.100.252.122 myservice.default.svc.cluster.local
```
We also set env variable in pod yaml
```
k exec -it myapp-pod -- printenv
Defaulted container "myapp-container" out of: myapp-container, init-myservice (init)
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=myapp-pod
FIRSTNAME=zphagia
```
