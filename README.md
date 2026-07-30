# Inference Optimisations

A reference collection of inference optimisation techniques — what each does, what it costs, and how to apply it.

## Index

| Optimisation | TTFT | Throughput | Load Time | Memory | Complexity |
|---|---|---|---|---|---|
| [vLLM PyTorch Compile Cache](optimisations/vllm-torch-compile-cache/) | ✅ Better | ✅ Better | ⚠️ Slower (first run) | ➡️ No change | 🟡 Medium |
| [vLLM InstantTensor](optimisations/vllm-instanttensor/) | ➡️ No change | ➡️ No change | ✅ Better | ➡️ No change | 🟡 Medium |

### Key

| Symbol | Meaning |
|---|---|
| ✅ | Improved |
| ⚠️ | Conditional or trade-off |
| ➡️ | No meaningful change |
| ❌ | Worse |
| 🟢 🟡 🔴 | Complexity: Low / Medium / High |

## Categories

These optimisations generally affect one or more of the following:

- **TTFT** — time to first token; how quickly the model starts responding
- **Throughput** — requests handled per second at a given concurrency
- **Load time** — how long the server takes to be ready after starting
- **Memory** — GPU or host memory footprint

Improving throughput typically increases latency. Trade-offs are called out prominently in each optimisation's README.
