If the app gets "stuck" in argocd, you can delete it using the following command

```
kubectl -n argocd patch application tcp-controller-app-pub1 \
  -p '{"metadata":{"finalizers":[]}}' --type=merge
```
