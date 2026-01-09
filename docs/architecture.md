# Architecture Overview

This project demonstrates a GitOps-driven Kubernetes platform.

## Key principles

- Git is the single source of truth
- No imperative deploys
- Platform ownership mindset
- Observability as a system

## High-level flow

Developer -> GitLab CI -> GitOps repo -> Argo CD -> Kubernetes
