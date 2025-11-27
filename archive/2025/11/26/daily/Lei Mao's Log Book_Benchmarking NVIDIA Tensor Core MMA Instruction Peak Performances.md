---
title: Benchmarking NVIDIA Tensor Core MMA Instruction Peak Performances
url: https://leimao.github.io/blog/Benchmarking-NVIDIA-Tensor-Core-MMA-Peak-Performances/
source: Lei Mao's Log Book
date: 2025-11-26
fetch_date: 2025-11-27T16:52:40.384611
---

# Benchmarking NVIDIA Tensor Core MMA Instruction Peak Performances

[![Lei Mao's Log Book](/images/favicon/android-chrome-512x512.png)](/)

[Lei Mao's Log Book](/)[Curriculum](/curriculum)[Blog](/blog)[Articles](/article)[Projects](/project)[Publications](/publication)[Readings](/reading)[Life](/life)[Essay](/essay)[Photography](/photography)[Archives](/archives)[Categories](/categories)[Tags](/tags)[FAQs](/faq)

# Benchmarking NVIDIA Tensor Core MMA Instruction Peak Performances

11-26-202511-26-2025 [blog](/blog/) 11 minutes read (About 1646 words)  visits

## Introduction

Whenever NVIDIA releases a new GPU architecture or a new GPU SKU, they will always advertise the peak AI performances of the GPU using the number of operations per second (OPS) metric, usually in units of TFLOPS (Tera Floating Point Operations Per Second) or TOPS (Tera Operations Per Second). The peak performances, especially those for the low precision data types, such as TF32, BF16, FP16, INT8, FP8, INT4, and FP4, come from the Tensor Core MMA (Matrix Multiply-Accumulate) instructions on the GPU.

Reproducing the advertised peak performances using HPC software or benchmarking tools does not always work, because it is completely possible that those software or benchmarking tools have not fully supported the architecture features of the GPU being tested. Even if the software is from NVIDIA itself, such as cuBLAS, the software might not have been fully optimized for the latest GPU architecture or SKU yet, when the GPU is delivered to end users.

Instead of relying on third-party software or benchmarking tools, a more reliable way to measure the peak performances of the Tensor Core MMA instructions is to write custom micro-benchmarks that directly invoke those MMA instructions. However, there are a lot of MMA instructions on NVIDIA GPUs and the performance of each instruction can vary significantly depending on the data types, matrix sizes, and other factors. Therefore, even if custom micro-benchmarks are created, it is still possible that some key MMA instructions are missed and the advertised peak AI performances cannot be reproduced.

In this blog post, I would like to demonstrate how to measure the peak performances of NVIDIA Tensor Core MMA instructions using CUTLASS and CuTe. The advertised peak AI performances of NVIDIA GPUs can be fully reproduced on my NVIDIA RTX 5080 GPU using this method. In addition, this can serve as a good reference for picking the right and performant Tensor Core MMA instructions for AI software development on NVIDIA GPUs.

## CuTe MMA Performance Benchmark

### CuTe MMA Atoms

Most of the widely used and performant NVIDIA Tensor Core MMA instructions are wrapped as MMA Atoms in CuTe and saved in the directory of [`include/cute/arch`](https://github.com/NVIDIA/cutlass/tree/v4.2.1/include/cute/arch) for different GPU architectures. For example, the SM80 FP16 Tensor Core instruction [`mma.sync.aligned.m16n8k16.row.col.f16.f16.f16.f16`](https://github.com/NVIDIA/cutlass/blob/v4.2.1/include/cute/arch/mma_sm80.hpp#L92) is wrapped as the `SM80_16x8x16_F16F16F16F16_TN` MMA Atom in CuTe as shown below.

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 ``` | ``` // MMA 16x8x16 TN struct SM80_16x8x16_F16F16F16F16_TN {   using DRegisters = uint32_t[2];   using ARegisters = uint32_t[4];   using BRegisters = uint32_t[2];   using CRegisters = uint32_t[2];    CUTE_HOST_DEVICE static void   fma(uint32_t      & d0, uint32_t      & d1,       uint32_t const& a0, uint32_t const& a1, uint32_t const& a2, uint32_t const& a3,       uint32_t const& b0, uint32_t const& b1,       uint32_t const& c0, uint32_t const& c1)   { #if defined(CUTE_ARCH_MMA_SM80_ENABLED)     asm volatile(       "mma.sync.aligned.m16n8k16.row.col.f16.f16.f16.f16 "       "{%0,  %1},"       "{%2,  %3,  %4,  %5},"       "{%6,  %7},"       "{%8,  %9};\n"       : "=r"(d0), "=r"(d1)       :  "r"(a0),  "r"(a1),  "r"(a2),  "r"(a3),          "r"(b0),  "r"(b1),          "r"(c0),  "r"(c1)); #else     CUTE_INVALID_CONTROL_PATH("Attempting to use SM80_16x8x16_F16F16F16F16_TN without CUTE_ARCH_MMA_SM80_ENABLED"); #endif   } }; ``` |

### Dummy MMA Kernel for Performance Benchmark

Implementing a high-performance MMA kernel that achieves the peak performances is usually not a trivial task. However, for the purpose of measuring the peak performances of the MMA instructions, we can implement a dummy MMA kernel that simply invokes the MMA instructions in a loop without having to load or store any data between the processor and the global memory. In this way, we can isolate the performance of the MMA instructions themselves without being affected by memory bandwidth or other factors. For example, to execute the SM80 MMA instructions in a loop, we can implement a generic MMA benchmark kernel using template expansion as shown below.

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 69 70 71 72 73 74 75 76 77 78 79 80 81 82 83 84 85 86 87 88 89 90 91 92 93 94 95 96 97 98 99 100 ``` | ``` // Template helpers to determine array sizes template <typename T> struct ArraySize;  template <typename T, size_t N> struct ArraySize<T[N]> {     static constexpr size_t value{N}; };  // Generic MMA benchmark kernel using template expansion template <typename MMA, size_t NUM_ITERS> __global__ void benchmark_mma_kernel() {     typename MMA::DRegisters d{};     typename MMA::ARegisters a{};     typename MMA::BRegisters b{};     typename MMA::CRegisters c{};      // Initialize with non-zero values to prevent optimization     constexpr size_t D_SIZE{ArraySize<typename MMA::DRegisters>::value};     constexpr size_t A_SIZE{ArraySize<typename MMA::ARegisters>::value};     constexpr size_t B_SIZE{ArraySize<typename MMA::BRegisters>::value};     constexpr size_t C_SIZE{ArraySize<typename MMA::CRegisters>::value};  #pragma unroll     for (size_t idx{0}; idx < A_SIZE; ++idx)         a[idx] = 1; #pragma unroll     for (size_t idx{0}; idx < B_SIZE; ++idx)         b[idx] = 1; #pragma unroll     for (size_t idx{0}; idx < C_SIZE; ++idx)         c[idx] = 1;  #pragma unroll 1     for (size_t i{0}; i < NUM_ITERS; ++i)     {         // Call fma with expanded parameters based on array sizes         if constexpr (D_SIZE == 2 && A_SIZE == 2 && B_SIZE == 1 && C_SIZE == 2)         {             // Pattern: SM80_16x8x8_F16F16F16F16_TN             MMA::fma(d[0], d[1], a[0], a[1], b[0], c[0], c[1]);         }         else if constexpr (D_SIZE == 2 && A_SIZE == 4 && B_SIZE == 2 &&                            C_SIZE == 2)         {             // Pattern: SM80_16x8x16_F16F16F16F16_TN             MMA::fma(d[0], d[1], a[0], a[1], a[2], a[3], b[0], b[1], c[0],                      c[1]);         }         else if constexpr (D_SIZE == 4 && A_SIZE == 2 && B_SIZE == 1 &&                            C_SIZE == 4)         {             // Pattern: SM80_16x8x8_F32F16F16F32_TN,             // SM80_16x8x4_F32TF32TF32F32_TN             MMA::fma(d[0], d[1], d[2], d[3], a[0], a[1], b[0], c[0], c[1], c[2],                      c[3]);         }         else if constexpr (D_SIZE == 4 && A_SIZE == 4 && B_SIZE == 2 &&                            C_SIZE == 4)         {             // Pattern: SM80_16x8x16_F32F16F16F32_TN,             // SM80_16x8x8_F32TF32TF32F32_TN             MMA::fma(d[0], d[1], d[2], d[3], a[0], a[1], a[2], a[3], b[0], b[1],                      c[0], c[1], c[2], c[3]);         }         else if constexpr (D_SIZE == 2 && A_SIZE == 1 && B_SIZE == 1 &&                            C_SIZE == 2)         {             // Pattern: SM80_8x8x16_S32S8S8S32_TN and similar INT8/INT4 variants             MMA::fma(d[0], d[1], a[0], b[0], c[0], c[1]);         }         else if constexpr (D_SIZE == 4 && A_SIZE == 1 && B_SIZE == 1 &&                            C_SIZE == 4)         {             // Pattern: SM80_16x8x4_F32BF1...