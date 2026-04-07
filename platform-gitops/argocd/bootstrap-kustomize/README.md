# Kustomize Bootstrap Variant

This directory expresses the same bootstrap layer as `argocd/bootstrap/`, but with Kustomize used only to factor out the shared `Application` boilerplate.

Use this when you want:

- less repeated Argo CD control-plane YAML
- the same rendered bootstrap objects
- no additional Helm wrapper layer for `Application` or `ApplicationSet` resources

The generated resources are still:

- `app-projects`
- `app-argocd-core`
- `app-clusters`
- `app-platform-appsets`
- `app-workload-appsets`
