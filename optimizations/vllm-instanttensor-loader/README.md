# vllm-instanttensor-loader

![TTFT](https://img.shields.io/badge/TTFT-Unchanged-lightgrey)
![Throughput](https://img.shields.io/badge/Throughput-Unchanged-lightgrey)
![Load Time](https://img.shields.io/badge/Load%20Time-Improved-brightgreen)
![Memory](https://img.shields.io/badge/Memory-Unchanged-lightgrey)
![Complexity](https://img.shields.io/badge/Complexity-Unchanged-lightgrey)

<!--
Badge colours:
  brightgreen = improved
  lightgrey   = no meaningful change
  orange      = trade-off or conditional
  red         = worse / high complexity
  yellow      = medium complexity
  green       = low complexity
-->

> ⚠️ **Trade-offs**
> - Requires very high storage speeds (>= 5GB/s).
> - Supports CUDA devices and PyTorch tensors only.
> - Requires additions to the default RHAII VLLM image.

---

## What this does

Instanttensor is a tensor loader designed to maximise I/O throughput, utilising modern high storage bandwidth, to reduce the time to model startup. 

## Before / After

```
  BEFORE (safetensors)

   ┌─────────────┐    ┌──────────────┐    ┌────────────────┐
   │  GPU Memory │    │  CPU Memory  │    │      Disk      │
   │             │    │  (page cache)│    │                │
   │   ●         │    │              │    │  ●  ●  ●  ●    │
   │      ● ◄────┼────┼─ ●      ◄────┼────┼─ ●             │
   │   ●         │    │              │    │     ●  ●  ●    │
   └─────────────┘    └──────────────┘    └────────────────┘
                        sequential, one tensor at a time


  AFTER (InstantTensor: Direct I/O + Pipelining)

   ┌─────────────┐                        ┌────────────────┐
   │  GPU Memory │                        │      Disk      │
   │             │                        │                │
   │   ● ◄───────┼────────────────────────┼─ ●             │
   │   ● ◄───────┼────────────────────────┼─ ●  ●  ●  ●    │
   │   ● ◄───────┼────────────────────────┼─ ●             │
   └─────────────┘                        └────────────────┘
                   no page cache; multiple tensors in-flight
```

## How to apply

1. Build a new vLLM image based on the default RHAII vLLM CUDA image using the provided [Containerfile](artifacts/Containerfile).

```bash
$ IMAGE_REGISTRY=""
$ podman build -t ${IMAGE_REGISTRY}/vllm-cuda-rhel9-instanttensor artifacts/
$ podman push ${IMAGE_REGISTRY}/vllm-cuda-rhel9-instanttensor:latest
```

2. As this relies on high speed storage, the model will have to be loaded from PVC or S3. The [load-model-to-pvc.sh](artifacts/load-model-to-pvc.sh) script has been provided to help you.

```bash
# Below provided for example.
NAMESPACE="model-deployment"
PVC_NAME="pvc-qwen35-35b"
MODEL_URL="hf://Qwen/Qwen3.5-35B-A3B"
PVC_SIZE="80Gi"
STORAGE_CLASS="my-fast-storageclass"

artifacts/load-model-to-pvc.sh -n ${NAMESPACE} -p ${PVC_NAME} -m ${MODEL_URL} -s ${PVC_SIZE} -c ${STORAGE_CLASS}
```

3. Deploy a model via RHOAI as usual or, example deployment files can be found in [artifacts/inferenceService](artifacts/inferenceService) or [artifacts/llmInferenceService](artifacts/llmInferenceService/).

4. Edit the `llmInferenceService` or `inferenceService` object to specify the following configurations.

```yaml
kind: LLMInferenceService
...
spec:
  template:
    containers:
      ...
      args:
        - '--load-format instanttensor'
      image: '${IMAGE_REGISTRY}/vllm-cuda-rhel9-instanttensor:latest'
```

```yaml
kind: InferenceService
...
spec:
  predictor:
    model:
      ...
      args:
        - '--load-format=instanttensor'
      image: '${IMAGE_REGISTRY}/vllm-cuda-rhel9-instanttensor:latest'
```

See [`artifacts/`](artifacts/) for ready-to-use manifests and config files.

## Test Environment

### Platform
- OpenShift 4.19 on AWS
- RHOAI 3.4
- LocalStorage Operator

### Model Serving Details
- [Qwen3.5-35B-A3B](https://huggingface.co/Qwen/Qwen3.5-35B-A3B)
- 2x NVIDIA L40S (single node)

## Results

TBD

## Further reading

- [InstantTensor GitHub Repo](https://github.com/scitix/InstantTensor)
- [vLLM Documentation](https://docs.vllm.ai/en/latest/models/extensions/instanttensor) 
