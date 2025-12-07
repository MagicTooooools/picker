---
title: NVIDIA Tensor Core TN Layout MMA Instruction
url: https://leimao.github.io/blog/NVIDIA-Tensor-Core-MMA-Instruction-TN-Layout/
source: Lei Mao's Log Book
date: 2025-12-06
fetch_date: 2025-12-07T03:28:24.648324
---

# NVIDIA Tensor Core TN Layout MMA Instruction

[![Lei Mao's Log Book](/images/favicon/android-chrome-512x512.png)](/)

[Lei Mao's Log Book](/)[Curriculum](/curriculum)[Blog](/blog)[Articles](/article)[Projects](/project)[Publications](/publication)[Readings](/reading)[Life](/life)[Essay](/essay)[Photography](/photography)[Archives](/archives)[Categories](/categories)[Tags](/tags)[FAQs](/faq)

# NVIDIA Tensor Core TN Layout MMA Instruction

12-06-202512-06-2025 [blog](/blog/) 16 minutes read (About 2389 words)  visits

## Introduction

NVIDIA Tensor Cores are specialized hardware units designed to accelerate matrix operations, particularly matrix multiplications, which are fundamental to many deep learning and high-performance computing applications. Tensor Cores utilize Matrix-Matrix Accumulate (MMA) instructions to perform these operations efficiently. However, most of the NVIDIA Tensor Core MMA instructions are optimized for a specific data layout known as TN layout, where the input matrix $A$ is stored in row-major order and input matrix $B$ is stored in column-major order.

In this article, I would like to discuss why TN layout is the most optimized layout for GEMM problems across different hardware architectures, how NVIDIA Tensor Core MMA instructions are designed specifically for TN layout, and how non-TN layout GEMM solutions can be implemented using these TN layout MMA instructions.

## GEMM and MMA Layouts

GEMM (General Matrix-Matrix Multiplication) computes matrix multiplications using different data layouts for an input matrix $A$ of shape $(M, K)$ and an input matrix $B$ of shape $(K, N)$, resulting in an output matrix $C$ of shape $(M, N)$. In the context of this article, we ignore the accumulation of matrix $C$ in GEMM problems.

MMA (Matrix-Matrix Accumulate) instructions are low-level instructions that perform matrix multiplications on small sub-matrices (tiles) of the input matrices $A$ and $B$, producing a sub-matrix (tile) of the output matrix $C$. MMA instructions are often used to accelerate GEMM operations on NVIDIA GPUs when using Tensor Cores.

There are four different layouts for GEMM problems as well as for MMA instructions:

1. TN: Matrix $A$ is stored in row-major and matrix $B$ is stored in column-major.
2. NT: Matrix $A$ is stored in column-major and matrix $B$ is stored in row-major.
3. NN: Both matrix $A$ and matrix $B$ are stored in column-major.
4. TT: Both matrix $A$ and matrix $B$ are stored in row-major.

The resulting matrix $C$ is always stored in column-major format. If the user wants to store the resulting matrix $C$ in row-major format, there are ways to translate the GEMM problems to the existing one described above. Please refer to my previous article [“cuBLAS GEMM API Usages for Column-Major and Row-Major Matrices”](/blog/cuBLAS-Transpose-Column-Major-Relationship/) for more details.

## MMA Instruction Performances of Different Layouts

Most of the NVIDIA Tensor Core MMA instructions, especially those on newer GPU architectures, such as SM80, SM90, SM100, and SM120, are specific to TN layout. There are no direct MMA instructions for other layouts such as NT, NN, and TT for those GPU architectures.

The only platform, as far as I know, that provides MMA instructions for all four layouts is the SM70 (Volta) architecture. Below are the benchmark results of different SM70 FP16 MMA instructions using [CuTe MMA Benchmark](/blog/Benchmarking-NVIDIA-Tensor-Core-MMA-Peak-Performances/) on an NVIDIA RTX 5080 GPU (SM120).

| MMA Instruction | Accumulation Datatype | Throughput (TOPS) |
| --- | --- | --- |
| `SM70_8x8x4_F16F16F16F16_TN` | FP16 | 95.499 |
| `SM70_8x8x4_F16F16F16F16_NT` | FP16 | 13.971 |
| `SM70_8x8x4_F16F16F16F16_NN` | FP16 | 30.857 |
| `SM70_8x8x4_F16F16F16F16_TT` | FP16 | 19.596 |
| `SM70_8x8x4_F32F16F16F32_TN` | FP32 | 107.138 |
| `SM70_8x8x4_F32F16F16F32_NT` | FP32 | 15.362 |
| `SM70_8x8x4_F32F16F16F32_NN` | FP32 | 28.888 |
| `SM70_8x8x4_F32F16F16F32_TT` | FP32 | 28.578 |

We could see that among all four layouts of MMA instructions in the same “family”, the TN layout MMA instruction has the highest throughput, which is about 3x to 7x faster than other layouts on SM120 architecture. It should be noted that, according to the [NVIDIA Parallel Thread Execution ISA](https://docs.nvidia.com/cuda/archive/12.6.2/parallel-thread-execution/#warp-level-matrix-load-instruction-ldmatrix), `mma.sync.m8n8k4`, the MMA instruction behind the CuTe MMA Atom wrappers, is optimized for target architecture SM70 and may have substantially reduced performance on other target architectures. Because I no longer have access to SM70 GPUs, I could not the true performance of SM70 MMA instructions on SM70 architecture.

## Why TN Layout Is Chosen as the Most Optimized for GEMM Problems?

The next question is, why TN layout is chosen as the most optimized and the only supported MMA instruction layout most of the time on NVIDIA GPU architectures?

In fact, it is not only NVIDIA GPU architectures that choose to optimize TN layout GEMM problems. Even before deep learning became a thing, GEMM problems are widely used in high-performance computing (HPC) applications, and TN layout is often the preferred layout for matrix multiplications in HPC as well. This is because TN layout allows for better memory access patterns and cache utilization, leading to improved performance. Even if we are just using single-threaded naive implementations on CPU without SIMD instructions, TN layout can still outperform other layouts due to better cache locality.

benchmark\_matmul\_layouts.cpp

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 69 70 71 72 73 74 75 76 77 78 79 80 81 82 83 84 85 86 87 88 89 90 91 92 93 94 95 96 97 98 99 100 101 102 103 104 105 106 107 108 109 110 111 112 113 114 115 116 117 118 119 120 121 122 123 124 125 126 127 128 129 130 131 132 133 134 135 136 137 138 139 140 141 142 143 144 145 146 147 148 149 150 151 152 153 154 155 156 157 158 159 160 161 162 163 164 165 166 167 168 169 170 ``` | ``` #include <chrono> #include <cstddef> #include <iomanip> #include <iostream> #include <random> #include <string> #include <vector>  void fill_random(std::vector<int>& mat) {     std::mt19937 gen(42);     std::uniform_int_distribution<int> dist(-100, 100);     for (auto& x : mat)     {         x = dist(gen);     } }  // TN: A row-major, B col-major, C col-major void matmul_TN(std::vector<int> const& A, std::vector<int> const& B,                std::vector<int>& C, size_t M, size_t N, size_t K) {     for (size_t m{0}; m < M; ++m)     {         for (size_t n{0}; n < N; ++n)         {             long long sum{0};             for (size_t k{0}; k < K; ++k)             {                 sum += static_cast<long long>(A[m * K + k]) *                        B[n * K + k]; // A row-major, B col-major             }             C[n * M + m] = static_cast<int>(sum); // C col-major         }     } }  // NT: A col-major, B row-major, C col-major void matmul_NT(std::vector<int> const& A, std::vector<int> const& B,                std::vector<int>& C, size_t M, size_t N, size_t K) {     for (size_t m{0}; m < M; ++m)     {         for (size_t n{0}; n < N; ++n)         {             long long sum{0};             for (size_t k{0}; k < K; ++k)             {                 sum += static_cast<long long>(A[k * M + m]) *                        B[k * N + n]; // A col-major, B row-major             }             C[n * M + m] = static_cast<int>(sum); // C col-major         }     } }  // NN: A col-major, B col-major, C col-major void matmul_NN(std::vector<int> const& A, std::vector<int> const& B,                std::vector<int>& C, size_t M, size_t N, size_t K) {     for (size_t m{0}; m < M; ++m)     {         for (size_t n{0}; n < N; ++n)         {             long long sum{0};             for (size_t k{0}; k < K; ++k)             {  ...