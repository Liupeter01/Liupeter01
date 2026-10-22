# Hi 👋, I'm L00pback4o4

**C++17 · Graphics · GPU Computing · Systems Performance**

[Projects](https://github.com/Liupeter01?tab=repositories) · [Vulkan](https://github.com/Liupeter01/Vulkan-Platform) · [CoreForge](https://github.com/Liupeter01/coreforge) · [CPU Rendering](https://github.com/Liupeter01/Software-Rasterizer)

## About me

I'm Jonathan Liu, a C++ engineer specialising in **rendering, CPU/GPU performance and concurrent systems**. My work spans cache-aware data layouts, memory hierarchy, GPU execution, explicit synchronisation, resource lifetime management and profiler-guided optimisation.

I build performance-oriented systems across graphics, parallel computing and asynchronous networking, with projects covering Vulkan rendering, CPU rasterization and path tracing, CUDA kernels, lock-free concurrency and distributed services.

My interests include **graphics engineering, GPU computing, high-performance C++ and low-latency infrastructure**.

## Selected projects

### [Vulkan-Platform](https://github.com/Liupeter01/Vulkan-Platform) — asynchronous rendering & resource lifetime

- Redesigned resource uploads around state-driven residency, asynchronous transfer submission and completion-gated reclamation, reducing **worst-case upload-path frame time from 424 to 4.66 ms (~91×)**.
- Built graphics/compute/transfer execution paths using timeline semaphores, per-frame contexts and explicit resource dependencies; profiled execution with **Nsight Graphics, RenderDoc and Vulkan validation layers**.
- Implemented GPU-resident simulation for **16,384 particles**, using 64 × 256-thread workgroups and ping-pong buffers.

*Benchmark: RTX 3080 Ti Laptop GPU, Windows/MSVC Release. Worst-case upload-path frame timing, validation disabled.*

### [CoreForge](https://github.com/Liupeter01/coreforge) — CPU/GPU performance engineering

A C++ performance engineering framework covering CPU memory-access optimisation, CUDA kernels, sparse storage and concurrent data structures.

| Workload | Baseline → optimised | Optimisation |
| --- | ---: | --- |
| 1024² FP32 matrix multiplication | **97.93 → 11.58 ms (8.46×)** | Reordered strided accesses; 32-column OpenMP blocks |
| 8192² transpose | **63.28 → 19.58 ms (3.23×)** | 64 × 64 cache tiles |
| 8192² horizontal 17-tap blur | **15.72 → 3.46 ms (4.55×)** | Tiling and prefetching |
| Xyx vertical-blur family | **25.74 → 15.91 ms (1.62×)** | SIMD, prefetching and interleaved accumulators |
| 1024² convolution, 16² kernel | **111.08 → 42.55 ms (2.61×)** | 16 × 16 output tiles; four independent reduction chains |

*Benchmark: Apple M5 Pro, macOS arm64 Release, 10-run wall-time medians with random interleaving, 7 October 2026. OpenMP: 15 threads. Convolution: single-threaded. Y-major tiled vertical blur: 12.34 ms.*

- Engineered a **four-pass, 256-bin CUDA radix sort** using warp ballot/match/shuffle primitives, padded shared-memory access and a custom hierarchical prefix scan. Processed **500M uint32 keys in 360 ms aggregate kernel time (~1.39B keys/s)** on an RTX 3080 Ti Laptop GPU.
- Benchmarked bounded ring buffers and reference-counted linked queues across **100M handoffs**. The ring achieved median throughput of **14.4M / 10.9M / 4.80M handoffs/s** at 1P1C / 2P2C / 4P4C.
- Measured tail latency under **4P4C open-loop load at 1M handoffs/s**: ring/linked queue **p99 of 667 ns / 3.63 µs**, and **p99.9 of 64.83 / 439.50 µs**, across 999K samples.
- Applied **TSan, ASan/LLDB, UBSan and happens-before analysis** to resolve lost-item, signed-shift and memory-reclamation defects through consumed-state tracking, loser-node release, identity-aware rollback and unique deletion ownership.
- Implemented count/checksum validation, unique-value regression checks and numerical correctness testing across **19 matrix, blur, transpose and convolution variants**.

*Benchmark: Apple M5 Pro, macOS arm64 Release, ten-run medians. Open-loop handoff latency measured from scheduled push time to consumer completion, incorporating scheduling and backpressure.*

### [Software-Rasterizer](https://github.com/Liupeter01/Software-Rasterizer) — portable CPU rasterization & path tracing

- Optimised triangle traversal using **32 × 32 framebuffer tiles** and portable **SIMDe eight-pixel batches** for depth interpolation and perspective-correct weights.
- Reduced median draw time from **10.866 to 7.790 ms (28.3%, 1.395×)** at 1024 × 1024. Achieved **p95 / p99 of 8.487 / 9.406 ms** across 3,000 measured frames.
- Implemented **BVH-accelerated Monte Carlo path tracing** with next-event estimation, cosine-weighted sampling, Russian roulette, mirror reflection and dielectric materials.
- Developed **21 regression cases** covering geometry, visibility, interpolation, scalar/SIMDe consistency, sampling and light transport.

*Benchmark: Apple M5 Pro, macOS arm64 Release, serial scheduling, portable ARM SIMD. Static Spot mesh with 5,856 triangles, Phong shading, 13.9% pixel coverage at 1024². Draw-only timing after 100 warm-up frames per mode. Recorded 9 October 2026. [Method and conditions](https://github.com/Liupeter01/Software-Rasterizer#0x05-benchmarks).*

### [DIMS](https://github.com/Liupeter01/Distributed-Instant-Messaging-System-Development) — asynchronous C++ messaging

- Developed asynchronous **Boost.Asio TCP services**, gRPC communication, a custom TLV binary protocol, a Qt client and connection-lifecycle management.
- Sustained **50,000 concurrent loopback TCP connections** and **42.8K request-response exchanges/s**, with **p50 / p99 RTT of 0.93 / 5.44 ms**.
- Completed **17.9M TCP-layer loopback exchanges** with zero observed exchange failures.

**Other systems work:** ARMv8-A bare-metal bring-up on Cortex-A72/Raspberry Pi 4B, including assembly entry, MMU configuration, timer/UART exception handling and cooperative context switching. Debugging with GDB/OpenOCD and UART traces.

## Tools I use

- **Languages:** C++17 (primary), C, CUDA C/C++, GLSL/Slang, Python, ARM assembly.
- **Graphics & compute:** Vulkan 1.3, SPIR-V, SIMDe/SSE/AVX2, CUDA warp primitives, oneTBB, OpenMP.
- **Concurrency & networking:** std::atomic, CAS, lock-free data structures, reference counting, hazard pointers, Boost.Asio, gRPC/Protobuf.
- **Measurement & debugging:** Google Benchmark, perf, Nsight Systems/Compute/Graphics, RenderDoc, ASan/TSan/UBSan, GDB/LLDB.
- **Build & systems:** Linux, CMake, Git, Docker, Redis, MySQL, Qt.

## Engineering approach

**Reproduce → Measure → Explain → Optimise → Validate.**

I approach performance engineering through profiling, hardware-aware design and controlled benchmarking. My work emphasises throughput, tail latency, memory efficiency, numerical correctness and concurrency safety.

I use repeatable workloads, explicit measurement boundaries and regression tests to evaluate optimisations and establish performance improvements.

<details>
<summary>GitHub activity</summary>

<p align="center">
  <img src="https://github-profile-summary-cards.vercel.app/api/cards/profile-details?username=Liupeter01&amp;theme=github_dark" alt="GitHub activity summary for Liupeter01" loading="lazy" />
</p>

</details>
