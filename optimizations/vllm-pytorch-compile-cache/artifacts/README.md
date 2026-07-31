# Artifacts

<!--
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

-->

## Structure

- `pvc.yaml` - Example OpenShift manifest for deploying a PVC.
- `servingRuntime.yaml` - Example OpenShift manifest for deploying a servingRuntime. Required to deploy either of the `inferenceService` manifests. 
- `inferenceService.yaml` - Manifest to deploy a "vanilla" `inferenceService`, without caching enabled. This serves an example of a "normal" deployment.
- `inferenceService-cached.yaml` - Manifest to deploy an `inferenceService`, with the caching optimisation. The `diff` between this and `inferenceService.yaml` shows the specific changes made. 

