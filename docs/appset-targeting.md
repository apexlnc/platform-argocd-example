# AppSet Targeting

This repository uses cluster labels as the placement API.

## Common Labels

- `env`: `nonprod` or `prod`
- `class`: `management` or `workload`
- `workload.gha-runners/enabled`: `"true"` when the runner estate should deploy
- `workload.devops/enabled`: `"true"` when the DevOps estate should deploy

## Targeting Model

- `appsets/platform.yaml` uses static component metadata plus cluster class
- `appsets/workloads.yaml` uses workload metadata plus cluster labels

The important detail is that the workload generator does not hardcode cluster URLs or maintain one `ApplicationSet` per repo.
