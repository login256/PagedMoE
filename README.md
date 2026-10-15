# PagedMoE

**Cross-Precision Expert Caching for Token-Level Dynamic Precision in SSD-Resident MoE Inference**

PagedMoE accelerates local Mixture-of-Experts (MoE) inference with SSD-resident experts. It combines token-level dynamic precision with cross-precision caching, reusing cached expert data across precision changes and loading only missing bitplanes. A single progressive representation supports direct CPU execution at 1-8 bits.

[Read the paper](./PagedMoE.pdf)

## Performance

Decode throughput (**tokens/s**) with a **24 GiB expert-cache budget**, **batch size 1**, and **2.5-bit average requested routed-expert precision** for PagedMoE:

| Model | Uniform4 | ik_llama.cpp (UD-IQ4) | PagedMoE | vs. Uniform4 | vs. ik_llama.cpp |
| --- | ---: | ---: | ---: | ---: | ---: |
| Qwen3.5-397B-A17B | 5.71 | 2.16 | **8.89** | **1.56x** | **4.12x** |
| GLM5.2 | 1.58 | 0.72 | **2.64** | **1.67x** | **3.67x** |

Uniform4 uses the same system with uniform 4-bit expert precision. The last two columns show PagedMoE's throughput speedup over each baseline.

**Hardware:** AMD Ryzen 9 9950X, NVIDIA RTX 5090, 64 GB RAM, and a PCIe 5.0 NVMe SSD. Comparisons use matched expert-cache budgets; total host memory differs across runtimes. See the paper for detailed memory accounting.

## Model Quality

At an average of 2.5 bits, PagedMoE retains quality close to GPTQ-INT4 on the evaluated Qwen3.5 tasks. On GLM5.2, it matches or exceeds MXFP4 on four of five evaluated benchmarks, with lower GSM8K-CoT accuracy.

## Code Availability

This repository currently hosts the paper. Code will be released here at a later date.
