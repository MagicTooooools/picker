---
title: NVIDIA NVML GPU Statistics
url: https://leimao.github.io/blog/NVIDIA-NVML-GPU-Statistics/
source: Lei Mao's Log Book
date: 2025-12-25
fetch_date: 2025-12-26T03:26:59.889449
---

# NVIDIA NVML GPU Statistics

[![Lei Mao's Log Book](/images/favicon/android-chrome-512x512.png)](/)

[Lei Mao's Log Book](/)[Curriculum](/curriculum)[Blog](/blog)[Articles](/article)[Projects](/project)[Publications](/publication)[Readings](/reading)[Life](/life)[Essay](/essay)[Photography](/photography)[Archives](/archives)[Categories](/categories)[Tags](/tags)[FAQs](/faq)

# NVIDIA NVML GPU Statistics

12-25-202512-25-2025 [blog](/blog/) 15 minutes read (About 2214 words)  visits

## Introduction

The NVIDIA official utility `nvidia-smi` provides a lot of useful information about the GPU. It is built on top of the NVIDIA Management Library (NVML), which provides a set of APIs for monitoring the statistics of NVIDIA GPUs. In practice, sometimes we would like to monitor the GPU statistics in our custom applications.

In this blog post, I would like to discuss how to use NVIDIA NVML library to monitor the GPU statistics and replicated `nvidia-smi dmon` in a custom C++ application.

## NVIDIA NVML GPU Statistics

### NVIDIA-SMI DMON

`nvidia-smi dmon` will display basic GPU statistics, including power (`pwr`), GPU temperature (`gtemp`), memory temperature (`mtemp`), GPU utilization (`sm`) (the percentage of time that at least one SM is being used), memory utilization (`mem`), encoder utilization (`enc`), decoder utilization (`dec`), JPEG utilization (`jpg`), OFA utilization (`ofa`), memory clock (`mclk`), and graphics clock (`pclk`). The following is an example of `nvidia-smi dmon` output:

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 ``` | ``` $ nvidia-smi dmon # gpu    pwr  gtemp  mtemp     sm    mem    enc    dec    jpg    ofa   mclk   pclk # Idx      W      C      C      %      %      %      %      %      %    MHz    MHz     0      8     42      -      1     11      0      0      0      0    405    502     0     14     43      -      0      1      0      0      0      0   7001   1492     0     15     43      -      0      1      0      0      0      0   7001   1492 ``` |

In addition, `nvidia-smi dmon` can also display additional GPU Performance Metrics (GPM) for Hopper and later GPUs. The following example shows how to display the GPMs for GPU activity (`gract`) (same as the `sm` metric), SM utilization (`smutil`) (the percentage of SMs that are actively being used), and FP16 activity (`fp16`).

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 ``` | ``` $ nvidia-smi dmon --gpm-metrics 1,2,13 # gpu    pwr  gtemp  mtemp     sm    mem    enc    dec    jpg    ofa   mclk   pclk      gract     smutil       fp16 # Idx      W      C      C      %      %      %      %      %      %    MHz    MHz      GPM:%      GPM:%      GPM:%     0     12     43      -      0      1      0      0      0      0   7001    682          -          -          -     0     12     43      -      0      1      0      0      0      0    810    532          -          -          -     0     11     43      -      2      7      0      0      0      0    810    495          1          1          0     0      9     43      -      1      7      0      0      0      0    810    502          9          8          0 ``` |

The other GPM metrics can be queried using `nvidia-smi dmon --help`.

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 69 70 71 72 73 74 75 76 77 78 79 80 81 82 83 84 85 86 ``` | ``` $ nvidia-smi dmon --help      GPU statistics are displayed in scrolling format with one line     per sampling interval. Metrics to be monitored can be adjusted     based on the width of terminal window. Monitoring is limited to     a maximum of 16 devices. If no devices are specified, then up to     first 16 supported devices under natural enumeration (starting     with GPU index 0) are used for monitoring purpose.     It is supported on Tesla, GRID, Quadro and limited GeForce products     for Kepler or newer GPUs under x64 and ppc64 bare metal Linux.     Note: On MIG-enabled GPUs, querying the utilization of encoder,     decoder, jpeg, ofa, gpu, and memory is not currently supported.      Usage: nvidia-smi dmon [options]      Options include:     [-i | --id]:          Comma separated Enumeration index, PCI bus ID or UUID     [-d | --delay]:       Collection delay/interval in seconds [default=1sec]     [-c | --count]:       Collect specified number of samples and exit     [-s | --select]:      One or more metrics [default=puc]                           Can be any of the following:                               p - Power Usage and Temperature                               u - Utilization                               c - Proc and Mem Clocks                               v - Power and Thermal Violations                               m - FB, Bar1 and CC Protected Memory                               e - ECC Errors and PCIe Replay errors                               t - PCIe Rx and Tx Throughput     [N/A | --gpm-metrics]: Comma-separated list of GPM metrics (no space in between) to watch                            Available metrics:                                Graphics Activity           = 1                                SM Activity                 = 2                                SM Occupancy                = 3                                Integer Activity            = 4                                Tensor Activity             = 5                                DFMA Tensor Activity        = 6                                HMMA Tensor Activity        = 7                                IMMA Tensor Activity        = 9                                DRAM Activity               = 10                                FP64 Activity               = 11                                FP32 Activity               = 12                                FP16 Activity               = 13                                PCIe TX                     = 20                                PCIe RX                     = 21                                NVDEC 0-7 Activity          = 30-37                                NVJPG 0-7 Activity          = 40-47                                NVOFA 0 Activity            = 50                                NVLink Total RX             = 60                                NVLink Total TX             = 61                                NVLink L0-17 RX             = 62,64,66,...,96                                NVLink L0-17 TX             = 63,65,67,...,97                                C2C TOTAL TX                = 100                                C2C TOTAL RX                = 101                                C2C DATA TX                 = 102                                C2C DATA RX                 = 103                                C2C LINK0-13 TOTAL TX       = 104,108,112,...,156                                C2C LINK0-13 TOTAL RX       = 105,109,113,...,157                                C2C LINK0-13 DATA TX        = 106,110,114,...,158                                C2C LINK0-13 DATA RX        = 107,111,115,...,159                                HOSTMEM CACHE HIT           = 160                                HOSTMEM CACHE MISS          = 161                                PEERMEM CACHE HIT           = 162                                PEERMEM CACHE MISS          = 163                                DRAM CACHE HIT              = 164                                DRAM CACHE MISS             = 165                                NVENC 0-3 Activity          = 166-169                                GR0-7 CTXSW CYCLES ELAPSED  = 170,175,180,...,205                                GR0-7 CTXSW CYCLES ACTIVE   = 171,176,181,...,206                                GR0-7 CTXSW REQUESTS        = 172,177,182,...,207                                GR0-7 CTXSW ACTIVE AVERAGE  = 173,178,183,...,208                                GR0-7 CTXSW ACTIVE PERCENT  = 174,179,184,...,209      [N/A | --gpm-options]: options of which level of GPM metrics to ...