# Ansible bootstrap

This directory bootstraps the Kubernetes cluster.

Responsibilities:
- OS preparation
- k3s installation
- base namespaces

Non-responsibilities:
- application delivery
- cluster add-ons

## Run

ansible-playbook site.yaml

Kubeconfig is fetched to ops/ansible/kubeconfig and rewritten to use the node IP.

## Validate

kubectl get nodes
kubectl get ns
kubectl version
