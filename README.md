# Inference Optimizations

A reference collection of inference optimization techniques — what each does, what it costs, and how to apply it.

## Index

| Optimisation | TTFT | Throughput | Load Time | Memory | Complexity |
|---|---|---|---|---|---|
| [vLLM PyTorch Compile Cache](optimisations/vllm-pytorch-compile-cache/) | ➡️ No change | ➡️ No change | ✅ Faster (after first run) | ➡️ No change | 🟢 Low |
| [vLLM InstantTensor](optimisations/vllm-instanttensor-loader/) | ➡️ No change | ➡️ No change | ✅ Better | ➡️ No change | 🟡 Medium |

### Key

| Symbol | Meaning |
|---|---|
| ✅ | Improved |
| ⚠️ | Conditional or trade-off |
| ➡️ | No meaningful change |
| ❌ | Worse |
| 🟢 🟡 🔴 | Complexity: Low / Medium / High |

## Categories

These optimizations generally affect one or more of the following:

- **TTFT** — time to first token; how quickly the model starts responding
- **Throughput** — requests handled per second at a given concurrency
- **Load time** — how long the server takes to be ready after starting
- **Memory** — GPU or host memory footprint

Improving throughput typically increases latency. Trade-offs are called out prominently in each optimisation's README.
