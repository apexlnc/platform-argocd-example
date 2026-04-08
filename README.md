# Platform GitOps Example

This repository now models a smaller Argo CD control-plane pattern:

- `projects/` are the real security and policy boundaries
- `workloads/` are metadata records, not Argo resources
- `appsets/` contain a small number of shared generators
- `values/` hold platform-wide defaults shared across environments
- `environments/` hold cluster inventory and platform overlays

The key design choice is to stop creating one bespoke `ApplicationSet` per workload or repo boundary. Instead:

- `AppProject` answers "what is allowed?"
- workload metadata answers "what should exist?"
- cluster inventory answers "where can it go?"
- shared `ApplicationSet`s answer "generate the Applications"

Start here:

- [docs/architecture.md](/Users/kevin/git/platform-argocd-example/docs/architecture.md)
- [docs/bootstrap-flow.md](/Users/kevin/git/platform-argocd-example/docs/bootstrap-flow.md)
- [projects/global-workloads.yaml](/Users/kevin/git/platform-argocd-example/projects/global-workloads.yaml)
- [workloads/gha-runners.yaml](/Users/kevin/git/platform-argocd-example/workloads/gha-runners.yaml)
- [appsets/workloads.yaml](/Users/kevin/git/platform-argocd-example/appsets/workloads.yaml)
