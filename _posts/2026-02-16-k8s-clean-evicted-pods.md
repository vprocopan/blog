To clean evicted pods run:

```bash
kubectl get pods -A | grep Evicted | awk '{print $1, $2}' | while read ns pod; do
  kubectl delete pod $pod -n $ns
done
```
