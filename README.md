# Ansible playbooks for kubernetes

This is based on the [official documentation](https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/).

## Changelog

* 2023-01-05 Kube version 1.24.17
* 2022-02-09 Kube version 1.21.5
* 2022-02-08 Kube version 1.20.15

## Upgrade kubernetes

1. Update `k8s_version` in the inventory file.

2. Upgrade first control plane node:

```
ansible-playbook -i inventory/preprod.yaml upgrade_k8s_first_master.yaml
```

3-N. Upgrade all others:

k8s masters, then workers, then app:

```
# set serial to 1 in play
ansible-playbook -i inventory/preprod.yaml -l app_k8s upgrade_k8s_other_node.yaml
ansible-playbook -i inventory/preprod.yaml -l app_masters upgrade_k8s_other_node.yaml
# set serial to 3 or more in play
ansible-playbook -i inventory/preprod.yaml -l app_workers upgrade_k8s_other_node.yaml
# set serial to 1 in play
ansible-playbook -i inventory/preprod.yaml -l app upgrade_k8s_other_node.yaml 
```

