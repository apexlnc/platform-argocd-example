# Argo CD Platform Layout Example

This repository contains a concrete starter layout for a self-managed, central Argo CD control plane in 2026-style multi-cluster GitOps.

It intentionally models two Git repos side by side:

- `platform-gitops/`: platform-owned Argo CD install, inventory, bootstrap, projects, and platform addon fanout
- `team-a-gitops/`: team-owned workload intent and per-application values

The operating model is:

- platform addons are driven from cluster labels via platform-owned `ApplicationSet`s
- business workloads are driven from `team repo workload files x matching clusters`
- team repos own workload definitions and values
- app source repos stay separate from GitOps repos
- bootstrap stays thin and admin-owned
- Argo CD itself is managed as a dedicated child app, not folded into the root app

This example is tuned for self-managed Argo CD on Amazon EKS:

- cluster secrets use the Kubernetes API server URL in `stringData.server`
- remote authentication uses `awsAuthConfig` in the cluster secret
- Argo CD itself is installed from the official `argo-cd` Helm chart
- hub-cluster auth for Argo CD can be layered in via `platform-gitops/argocd/core/values.eks-irsa.example.yaml`

The cluster inventory vocabulary in this example is intentionally small and Argo-oriented:

- labels: `env`, `region`, `team`, `type`, `ready`
- annotations: `account-id`

For a real EKS cluster secret, replace the placeholder API server URL, IAM role ARN, and `caData` with your cluster's actual values per Argo CD declarative setup.

Optional Argo CD hardening overlays live under `platform-gitops/argocd/core/`:

- `values.oidc.example.yaml`: direct OIDC SSO and RBAC example
- `values.repo-creds.github-app.example.yaml`: GitHub App repository credential template example
- `values.eks-irsa.example.yaml`: IRSA wiring for the Argo CD management role on the hub cluster

Managed-cluster onboarding steps for EKS live in `platform-gitops/argocd/clusters/README.md`.

Upstream guidance this layout follows:

- ApplicationSets with cluster labels are the recommended cluster bootstrapping pattern
- App-of-Apps is kept to the bootstrap layer only
- multiple sources are used only for narrow cases such as `chart + values repo`
- dedicated `AppProject`s are used instead of relying on `default`
