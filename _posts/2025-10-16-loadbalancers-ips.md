## All LoadBalancer Service external IPs, all namespaces:

```yaml
kubectl get svc -A -o jsonpath='{range .items[?(@.spec.type=="LoadBalancer")]}{.metadata.namespace}{"\t"}{.metadata.name}{"\t"}{range .status.loadBalancer.ingress[*]}{.ip}{.hostname}{" "}{end}{"\n"}{end}'
```
