# [Optimisation Name]

![TTFT](https://img.shields.io/badge/TTFT-Improved-brightgreen)
![Throughput](https://img.shields.io/badge/Throughput-Unchanged-lightgrey)
![Load%20Time](https://img.shields.io/badge/Load%20Time-Improved-brightgreen)
![Memory](https://img.shields.io/badge/Memory-Higher-orange)
![Complexity](https://img.shields.io/badge/Complexity-Medium-yellow)

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
> - [Most significant cost, upfront]
> - [e.g. higher memory, slower first-run, operational overhead, storage dependency]

---

## What this does

One or two plain sentences. Assume the reader knows what vLLM is but not this technique. No acronyms without explanation.

## Before / After

```
BEFORE
[describe the slow / inefficient path in plain ASCII]

AFTER
[describe the improved path]
```

## How to apply

See [`artifacts/`](artifacts/) for ready-to-use manifests and config files.

### Steps

Step-by-step. Call out any environment or storage prerequisites before the steps.

```bash
# example config or command
```

## Test Environment

Explanation of the environment. Call out anything unusual, or non-standard. This is what's consistent between tests, if several tests have been done.

### Platform
- OpenShift <> on <>
- RHOAI <>
- Additional Operator Info (i.e. RHCL, NFD, GPU, if important)

### Model Serving Details (if consistent)
- [Model link]
- GPU Info

## Results

What difference did this make in testing? If multiple (categories) of tests are done, call out what changes between them.

### Test 1

| Metric | Before | After | Delta |
|---|---|---|---|
[e.g. load time] | Xs | Xs | -X% |

### Test 2 (delete if N/A)

| Metric | Before | After | Delta |
|---|---|---|---|
[e.g. load time] | Xs | Xs | -X% |

## Further reading

- [link]
