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

- `inferenceService/` : Directory for storing example OpenShift manifests to deploy a model via InferenceService APIs.
- `llmInferenceService/` : Directory for storing example OpenShift manifests to deploy a model via LLMInferenceService APIs.
- `load-model-to-pvc.sh` : Helper script to load models from HuggingFace into a PVC on cluster.
- `Containerfile` : Containerfile that builds a vLLM container image with instanttensor.  
