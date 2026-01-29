## to add a node use:

```bash
ansible-playbook -i /inventory/inventory.ini --private-key /root/.ssh/id_ed25519 scale.yml --limit="node6,kube_control_plane" --become -vvv
```
