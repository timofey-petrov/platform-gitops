# Architecture Overview

This project demonstrates a GitOps-driven Kubernetes platform.

## Key principles

- Git is the single source of truth
- No imperative deploys
- Platform ownership mindset
- Observability as a system

## High-level flow

Developer -> GitLab CI -> GitOps repo -> Argo CD -> Kubernetes

## Cluster bootstrap (M1)

- k3s is provisioned via Ansible (day-1)
- single-node cluster for demo purposes
- no workload delivery via Ansible
