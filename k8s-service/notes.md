#Node port run successfully
```
controlplane:~/CKA-hands-on/k8s-service$ curl 172.30.1.2:30001
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
#All pods and services
```
controlplane:~/CKA-hands-on/k8s-service$ k get all
NAME                               READY   STATUS    RESTARTS   AGE
pod/my-app                         1/1     Running   0          43m
pod/nginx-deploy-c9d9f6c6c-5ttlc   1/1     Running   0          40m
pod/nginx-deploy-c9d9f6c6c-w9gbg   1/1     Running   0          40m
pod/nginx-deploy-c9d9f6c6c-x5fsh   1/1     Running   0          40m

NAME                        TYPE           CLUSTER-IP      EXTERNAL-IP              PORT(S)        AGE
service/cluster-ip-svc      ClusterIP      10.99.127.245   <none>                   80/TCP         24m
service/external-name-svc   ExternalName   <none>          my.database.meiot.live   <none>         8s
service/kubernetes          ClusterIP      10.96.0.1       <none>                   443/TCP        18d
service/lb-svc              LoadBalancer   10.100.142.80   <pending>                80:30212/TCP   5m12s
service/nodeport-svc        NodePort       10.99.34.37     <none>                   80:30001/TCP   38m

NAME                           READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-deploy   3/3     3            3           40m

NAME                                     DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-deploy-c9d9f6c6c   3         3         3       40m
```
