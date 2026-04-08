# GitHub Governance

Recommended governance for this repository:

- require pull requests for changes to `bootstrap/`, `projects/`, `appsets/`, and `workloads/`
- protect `main`
- require status checks from `.github/workflows/validate-gitops.yaml`
- assign `CODEOWNERS` around control-plane, project-policy, and workload metadata boundaries

Suggested review split:

- platform team owns `bootstrap/`, `projects/`, `appsets/`, and `environments/*/clusters/`
- workload owners co-own their specific `workloads/*.yaml` entries
