# Artifacts

Deployment-ready files for this optimisation. Drop anything here that someone would need to actually apply it — don't make them reconstruct it from prose in the README.

## What belongs here

| Type | Examples |
|---|---|
| Kubernetes / OpenShift manifests | `serving-runtime.yaml`, `inference-service.yaml`, `pvc.yaml` |
| Containerfiles | `Containerfile` |
| Helm values overrides | `values-override.yaml` |
| Kustomize patches | `kustomization.yaml`, patch files |
| Scripts | install or benchmark scripts |
| Config snippets | environment variable blocks, vLLM config fragments |

## Naming convention

Prefer descriptive names over generic ones:

```
artifacts/
├── serving-runtime.yaml          # good
├── pvc-rwx.yaml                  # good — clarifies access mode
└── config.yaml                   # too generic — rename it
```

Add a brief comment at the top of each file explaining any non-obvious values or prerequisites.
