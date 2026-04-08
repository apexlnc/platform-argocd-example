# Bootstrap Flow

1. Apply [bootstrap/control-plane/root-app.yaml](/Users/kevin/git/platform-argocd-example/bootstrap/control-plane/root-app.yaml) to the management cluster.
2. The root app syncs:
   - cluster secrets from `environments/*/clusters/`
   - `AppProject`s from `projects/`
   - shared generators from `appsets/`
3. `appsets/platform.yaml` fans platform components out to management or workload clusters.
   - shared defaults come from `values/platform/`
   - environment overrides come from `environments/<env>/platform/`
4. `appsets/workloads.yaml` combines:
   - workload metadata from `workloads/*.yaml`
   - registered Argo CD clusters
5. Argo CD reconciles the generated `Application`s into the target clusters.

The root app stays thin. It installs the control-plane primitives only.
