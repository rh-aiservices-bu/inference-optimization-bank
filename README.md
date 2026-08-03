# Inference Optimizations

A reference collection of inference optimization techniques for vLLM and llm-d. Answering the questions:

- What it does?
- What it benefits?
- What are the trade offs?
- How to implement it?

## Technique overview

The below table gives an overview of the optimization techniques, including their major benefit, trade off.

The complexity to implement each optimization is also shown, out of:

🟢 - Easy

🟠 - Medium

🔴 - Hard

| Optimisation | Benefit | Trade-off | Complexity |
|---|---|---|---|
| [vLLM PyTorch Compile Cache](optimizations/vllm-pytorch-compile-cache/) |  Model Load Time | - | 🟢 |
| [vLLM InstantTensor](optimizations/vllm-instanttensor-loader/) | Model Load Time | - | 🟠 |

## Files

- [Optimizations directory](optimizations/) : Where each optimization technique lives. An indiviudal README explaining the technique is in each directory. 
- [Templates directory](./templates/) : Contains templates to
- [optimization-index.csv](./optimization-index.csv) : A CSV containing every optimization, what it benefits, what it hinders and what it doesn't affect. Essentially a more verbose version of the table [above](./README.md#technique-overview).