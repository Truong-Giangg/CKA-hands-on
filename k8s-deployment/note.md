# Deployment create replica set, replica set create pods
```
$k set image deploy/nginx-deploy \
> nginx=nginx:1.9.1

$k rollout history deploy/nginx-deploy

$k rollout undo deploy/nginx-deploy
```
Auto create yaml file with imperative way:
```
$k create deploy deploy/nginx-new --image=nginx --dry-run=client -o yaml > deploy.yaml
