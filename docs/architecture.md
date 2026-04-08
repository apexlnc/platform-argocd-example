# Architecture

This layout assumes one central Argo CD control plane managing multiple clusters.

## Control Model

- `projects/` define runtime trust boundaries with `AppProject`
- `workloads/` define workload metadata records consumed by generators
- `appsets/platform.yaml` handles platform addons
- `appsets/workloads.yaml` handles workload estates from the metadata catalog
- `values/platform/` holds shared platform defaults
- `environments/*/clusters/` define cluster inventory and placement labels

## Why This Shape

The main correction is to stop encoding repo boundaries as handwritten `ApplicationSet`s. That pattern mixes policy, ownership, and generation in one place and scales badly.

The cleaner split is:

- policy in `AppProject`
- workload intent in metadata files
- placement in cluster labels
- generation in one shared `ApplicationSet`

## Placement

Cluster secrets are the placement API. Workload metadata records carry the label key/value they need, and the shared workload generator combines them with registered clusters.

## Global Workload Policy

[projects/global-workloads.yaml](/Users/kevin/git/platform-argocd-example/projects/global-workloads.yaml) is included as the common policy baseline. In a real deployment, Argo CD global project inheritance would be wired through Argo CD configuration so individual workload projects can stay thin.
