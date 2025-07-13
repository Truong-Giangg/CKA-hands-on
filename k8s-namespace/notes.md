#Create deployment on nginx-demo namespace
```
k create deployment nginx-deploy --image=nginx --replicas=3 -n nginx-demo
```
#Expose default deployment
```
k expose --name=svc-default deployment nginx-deploy --port 80
```
#Expose nginx-demo namespace deployment
```
k expose deployment nginx-deploy --name=svc-demo --port=80 -n nginx-demo
```
#Can not curl service name from nginx-demo namespace as normal
```
controlplane:~$ k get pod
NAME                           READY   STATUS    RESTARTS   AGE
nginx-deploy-c9d9f6c6c-4fcr2   1/1     Running   0          8m31s
nginx-deploy-c9d9f6c6c-crwb7   1/1     Running   0          8m31s
nginx-deploy-c9d9f6c6c-lsmcf   1/1     Running   0          8m31s
controlplane:~$ k exec -it nginx-deploy-c9d9f6c6c-4fcr2 -- sh
# 
# 
# curl svc-demo
curl: (6) Could not resolve host: svc-demo
```
#To access service name from nginx-demo namespace, we find the FQDN of pod
```
controlplane:~/CKA-hands-on/k8s-namespace$ k exec -it nginx-deploy-c9d9f6c6c-cz7b6 -n nginx-demo -- sh
# cat /etc/resolv.conf
search nginx-demo.svc.cluster.local svc.cluster.local cluster.local
nameserver 10.96.0.10
options ndots:5
```
#Access to namepace nginx-demo pods from default namespace
```
controlplane:~/CKA-hands-on/k8s-namespace$ k exec -it nginx-deploy-c9d9f6c6c-4fcr2 -- sh
# curl svc-demo.nginx-demo.svc.cluster.local
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```
