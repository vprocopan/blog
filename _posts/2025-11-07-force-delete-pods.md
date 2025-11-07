## To force delete all pods in a namespace
```
kubectl delete pods --all -n sentry --force --grace-period=0
```
