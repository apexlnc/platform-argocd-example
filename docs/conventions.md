# Conventions

## Naming

- `projects/<name>.yaml` for `AppProject`s
- `workloads/<name>.yaml` for workload metadata records
- `appsets/platform.yaml` for platform generation
- `appsets/workloads.yaml` for workload generation
- `values/platform/<component>.yaml` for shared platform defaults
- `environments/<env>/platform/<component>.yaml` for environment-specific platform values

## Scope

- `projects/` are policy boundaries
- `workloads/` are catalog inputs, not Argo resources
- `appsets/` are shared generation logic
- `values/` are shared defaults
- `environments/` are inventory and overlays

## Safety

- keep the root app thin
- do not use the `default` project for generated applications
- prefer adding workload metadata over creating another bespoke `ApplicationSet`
