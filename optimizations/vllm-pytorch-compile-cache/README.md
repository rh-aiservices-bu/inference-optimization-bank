# vllm-pytorch-compile-cache

![TTFT](https://img.shields.io/badge/TTFT-Unchanged-lightgrey)
![Throughput](https://img.shields.io/badge/Throughput-Unchanged-lightgrey)
![Load%20Time](https://img.shields.io/badge/Load%20Time-Improved-brightgreen)
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
> - Benefits only appear _after_ initial model deployment.
> - Cache can only be reused when model and environment stay the same between deployments.

---

## What this does

`torch.compile` is an integration of vLLM that will provide performance benefits to it. This is enabled by default when deploying to OpenShift, however compilation can slow down model start-up time. Caching the compiled artifacts allows subseqent deployments of the same model to happen faster.

## Before / After

```bash
   BEFORE                           AFTER

    ┌────────────────────────┐      ┌────────────────────────┐        ┌──────────────────┐
    │       vLLM Pod         │      │       vLLM Pod         │        │     Storage      │
    │                        │      │                        │        │                  │
    │   ┌────────────────┐   │      │   ┌────────────────┐   │        │                  │
    │   │  Model Start   │   │      │   │  Model Start   │   │        │                  │
    │   └───────┬────────┘   │      │   └───────┬────────┘   │        │                  │
    │           │            │      │           │            │        │                  │
    │           ▼            │      │           ▼            │        │                  │
    │   ┌────────────────┐   │      │   ┌────────────────┐   │        │   ┌──────────┐   │
    │   │ torch.compile  │   │      │   │ Retrieve Cache │◄──┼────────┼───│ .cache/  │   │
    │   │    (~28s)      │   │      │   │    (~4s)       │   │        │   │          │   │
    │   └───────┬────────┘   │      │   └───────┬────────┘   │        │   └──────────┘   │
    │           │            │      │           │            │        │                  │
    │           ▼            │      │           ▼            │        │                  │
    │   ┌────────────────┐   │      │   ┌────────────────┐   │        │                  │
    │   │ Model Started  │   │      │   │ Model Started  │   │        │                  │
    │   └────────────────┘   │      │   └────────────────┘   │        │                  │
    └────────────────────────┘      └────────────────────────┘        └──────────────────┘
```

## How to apply

See [`artifacts/`](artifacts/) for ready-to-use manifests and config files.

### Steps:

1. First you need to create a PVC. This will be mounted in the pod that deploys the model, and will be where the first deployment places the cache. You can create your own, or use the [manifest provided](artifacts/pvc.yaml).

  ```bash
  NAMESPACE=""
  oc apply -f artifacts/pvc.yaml -n $NAMESPACE
  ```

2. Deploy your model via the RHOAI UI. To enable caching, you need to add the following to the `inferenceService` object, post deployment.

```yaml
kind: InferenceService
spec:
  predictor:
    model:
      env:
        - name: VLLM_CACHE_ROOT
          value: /var/cache/vllm
      volumeMounts:
        - mountPath: /var/cache/vllm
          name: compile-cache
    volumes:
      - name: compile-cache
        persistentVolumeClaim:
          claimName: model-compilation-cache

```
The `inferenceService` manifests in [artifacts/](artifacts/) act as an example, or guidance if you want to add this to an already-deployed model. 

3. When deploying the first time, the predictor pod will have logs such as below.

```bash
[backends.py:371] Cache the graph of compile range (1, 2048) for later use
[backends.py:387] Compiling a graph for compile range (1, 2048) takes 23.55 s
[decorators.py:627] saved AOT compiled function to /var/cache/vllm/torch_compile_cache/...etc
[monitor.py:48] torch.compile took 38.16 s in total
```

4. By comparison, when restarting that pod (i.e. redeploying the model), the logs will instead look like this;

```bash
[backends.py:988] Using cache directory: /var/cache/vllm/torch_compile_cache/...etc for vLLM's torch.compile
[backends.py:1048] Dynamo bytecode transform time: 3.55 s
[backends.py:284] Directly load the compiled graph(s) for compile range (1, 2048) from the cache, took 1.963 s
[monitor.py:48] torch.compile took 6.00 s in total
```

## Test Environment

### Platform
- OpenShift 4.19 on AWS
- RHOAI 3.4
- gp3 Storage

### Model Serving Details
- Models Used:
  - [Qwen3.5-4B](https://huggingface.co/Qwen/Qwen3.5-4B)
  - [Qwen3.5-27B](https://huggingface.co/Qwen/Qwen3.5-27B)
- 2x NVIDIA L40S (single node)

## Results

Several models were tested. 
- In the "Before" Metric, the model is deployed without a cache and has to run `torch.compile`
- The "After" metric measures the `torch.compile` time when reading from the cache.

### Test 1 - Qwen3.5-4B

| Metric | Before | After | Delta |
|---|---|---|---|
| torch.compile time | 28.36s | 3.55s | -87% |

### Test 2 - Qwen3.5-27B (TP=2)

| Metric | Before | After | Delta |
|---|---|---|---|
torch.compile time | 38.16s | 6.00s | -84% |

## Further reading

- vLLM Blog: ["Introduction to torch.compile and how it works in vLLM"](https://vllm.ai/blog/2025-08-20-torch-compile)
