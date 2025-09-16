## If a namespace in K8s gets stuck terminating, it means finalizers are not removed. It can be done with the following command.

```
kubectl get ns opentelemetry-kube-stack -o json | jq '.spec.finalizers=[]' | kubectl replace --raw "/api/v1/namespaces/opentelemetry-kube-stack/finalize" -f - 
```



## If argocd application gets stuck

kubectl -n argocd patch application opentelemetry-kube-stack \
  -p '{"metadata":{"finalizers":[]}}' --type=merge