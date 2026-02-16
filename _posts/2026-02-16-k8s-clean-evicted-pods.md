To clean evicted pods run:

```bash
kubectl get pods -A -o json \
| jq -r '.items[]
  | select(.status.reason=="Evicted")
  | "\(.metadata.namespace) \(.metadata.name)"' \
| while read ns pod; do
    kubectl delete pod -n "$ns" "$pod" --grace-period=0 --force
  done
```
