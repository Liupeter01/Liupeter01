<h1 align="center">Hi 👋, I'm L00pback4o4</h1>
<h3 align="center">C++17 · Graphics · GPU Computing · Systems Performance</h3>

<p align="center">
  <a href="https://github.com/Liupeter01?tab=repositories">Projects</a>
  · <a href="https://github.com/Liupeter01/Vulkan-Platform">Vulkan</a>
  · <a href="https://github.com/Liupeter01/coreforge">CoreForge</a>
  · <a href="https://github.com/Liupeter01/Software-Rasterizer">CPU Rendering</a>
</p>

### About me

I'm Jonathan Liu, a C++ programmer focused on **rendering, CPU/GPU performance and concurrent systems**. I work from data layout and memory hierarchy through explicit synchronisation, resource lifetime and profiler-guided optimisation.

My projects include a Vulkan renderer, a portable CPU rasterizer/path tracer, CUDA sorting kernels, and asynchronous C++ services. I'm interested in **graphics, GPU computing and low-latency systems roles**, including trading infrastructure—not claiming production trading-system experience.

### Selected projects

#### [Vulkan-Platform](https://github.com/Liupeter01/Vulkan-Platform) — asynchronous rendering & resource lifetime

- Replaced blocking, repeated uploads with state-driven residency, asynchronous transfer submission and completion-gated reclamation; historical same-scene **worst-case frame time fell from 424 to 4.66 ms (~91×)**.
- Built graphics/compute/transfer execution paths using timeline semaphores, per-frame contexts and explicit resource dependencies; inspected execution with **Nsight Graphics, RenderDoc and Vulkan validation layers**.
- Implemented GPU-resident simulation for **16,384 particles**, with 64 × 256-thread workgroups and ping-pong buffers.

*Timing scope: historical RTX 3080 Ti Laptop GPU, Windows/MSVC Release; validation disabled for timing. The 91× figure compares worst-case upload-path frame times, not steady-state FPS.*

#### [CoreForge](https://github.com/Liupeter01/coreforge) — CPU/GPU performance engineering

**CoreForge replaces the earlier libHPC name.** The project combines CPU memory-access experiments, CUDA kernels, sparse storage and concurrent-container prototypes.

| Workload | Baseline → optimised | What changed |
| --- | ---: | --- |
| 1024² FP32 matrix multiplication | **97.93 → 11.58 ms (8.46×)** | Reordered strided inner-loop accesses; 32-column OpenMP blocks |
| 8192² transpose | **63.28 → 19.58 ms (3.23×)** | 64 × 64 cache tiles |
| 8192² horizontal 17-tap blur | **15.72 → 3.46 ms (4.55×)** | Tiling and prefetching |
| Xyx vertical-blur family | **25.74 → 15.91 ms (1.62×)** | SIMD, prefetching and interleaved accumulators |
| 1024² convolution, 16² kernel | **111.08 → 42.55 ms (2.61×)** | 16 × 16 output tiles; four independent reduction chains |

*CPU scope: Apple M5 Pro, macOS arm64 Release, 10-run wall-time medians with random interleaving, 7 October 2026. OpenMP uses 15 threads; convolution is single-threaded. Xyx is compared with its own baseline; a y-major tiled vertical variant was faster at 12.34 ms. These are comparisons with my own implementations, not vendor BLAS.*

- Built a **four-pass, 256-bin CUDA radix sort** using warp ballot/match/shuffle primitives, padded shared-memory paths and a custom hierarchical scan: **500M uint32 keys in 360 ms aggregate kernel time (~1.39B keys/s)** on an RTX 3080 Ti Laptop GPU. This historical result is kernel-only, not end-to-end transfer/allocation time.
- Benchmarked a bounded ring and an allocating reference-counted linked queue over **100M handoffs**. The ring reached median **14.4M / 10.9M / 4.80M handoffs/s** at 1P1C / 2P2C / 4P4C in ten M5 Pro Release runs.
- In a separate **4P4C open-loop** test at **1M handoffs/s offered load**, ring/linked **p99 was 667 ns / 3.63 µs**, with p99.9 of 64.83 / 439.50 µs (999K samples). Latency runs from planned push time to consumer completion and includes producer lateness/backpressure; it is not one queue-call latency.
- Used **TSan, ASan/LLDB, UBSan and happens-before analysis** to repair observed lost-item, signed-shift and reclamation defects. Fixes included consumed-state marking, correct loser-node release, identity-aware empty-pop rollback and unique deletion ownership.
- Added count/checksum checks to timing tests and unique-value checks to regressions; **19 matrix/blur/transpose/convolution variants passed numerical checks** on the benchmark inputs. Passing tests are evidence for exercised cases, not a general race/ABA-freedom proof.

#### [Software-Rasterizer](https://github.com/Liupeter01/Software-Rasterizer) — portable CPU rasterization & path tracing

- Reorganised triangle traversal into **32 × 32 framebuffer tiles** and added portable **SIMDe eight-pixel batches** for depth interpolation and perspective-correct weights. Coverage tests and fragment shading are not claimed to be fully vectorised.
- Same-machine scalar/SIMDe comparison: **median draw time 10.866 → 7.790 ms**, a **28.3% reduction (1.395×)** at 1024 × 1024. SIMDe **p95 / p99: 8.487 / 9.406 ms** over 3,000 measured frames.
- Built BVH-based Monte Carlo path tracing with next-event estimation, cosine-weighted sampling, Russian roulette and mirror/dielectric materials.
- Added **21 regression cases** covering geometry, visibility, interpolation, scalar/SIMDe agreement, sampling and light transport.

*Recorded 9 October 2026: Apple M5 Pro, macOS arm64 Release, TBB disabled/serial scheduling, portable ARM SIMD. Static Spot mesh, 5,856 input triangles, Phong shading, 13.9% pixel coverage at 1024²; 100 warm-up frames per mode per run. Timing surrounds draw(), excludes loading/display/file output, and is not full-application FPS. Older Intel figures are not this comparison's baseline. [Method and conditions](https://github.com/Liupeter01/Software-Rasterizer#0x05-benchmarks).*

#### [DIMS](https://github.com/Liupeter01/Distributed-Instant-Messaging-System-Development) — asynchronous C++ messaging

- Built Boost.Asio TCP services, gRPC communication, a custom TLV binary protocol, a Qt client and connection-lifecycle management.
- Historical **isolated TCP-layer** benchmark: **50,000 concurrent loopback connections**, **42.8K request-response exchanges/s**, **p50 / p99 RTT of 0.93 / 5.44 ms**, with no failed exchanges observed in the 17.9M-exchange run.
- These are connection-layer loopback results—not full-service throughput, one-way message rate or production-network latency.

**Other systems work:** ARMv8-A bare-metal bring-up on Cortex-A72/Raspberry Pi 4B: assembly entry, MMU mappings, timer/UART exceptions and cooperative context switching, debugged with GDB/OpenOCD and UART traces.

### Tools I use

- **Languages:** C++17 (primary), C, CUDA C/C++, GLSL/Slang, Python, ARM assembly.
- **Graphics & compute:** Vulkan 1.3, SPIR-V, SIMDe/SSE/AVX2, CUDA warp primitives, oneTBB, OpenMP.
- **Concurrency & networking:** std::atomic, CAS, reference-counted/hazard-pointer experiments, Boost.Asio, gRPC/Protobuf.
- **Measurement & debugging:** Google Benchmark, perf, Nsight Systems/Compute/Graphics, RenderDoc, ASan/TSan/UBSan, GDB/LLDB.
- **Build & systems:** Linux, CMake, Git, Docker; Redis/MySQL and Qt in the messaging project.

### Engineering approach

**Reproduce → measure → explain → change → validate.**

I keep workload size, hardware, timing boundaries and comparison baselines next to performance claims. Throughput, tail latency, numerical correctness and memory-lifetime checks answer different questions; I test them separately.

<details>
<summary>GitHub activity</summary>

<p align="center">
  <img src="https://github-profile-summary-cards.vercel.app/api/cards/profile-details?username=Liupeter01&amp;theme=github_dark" alt="GitHub activity summary for Liupeter01" loading="lazy" />
</p>

</details>
