# xpu-forge

A workshop for reverse-engineering accelerator silicon.

**xPU** is shorthand for the everything-not-a-CPU: GPUs, NPUs, TPUs,
DPUs, IPUs, AI engines, vector extensions, embedded MCUs inside larger
chips. Most of them ship with proprietary, undocumented, or
under-documented instruction sets. xpu-forge builds the
[Ghidra](https://github.com/NationalSecurityAgency/ghidra) processor
modules — SLEIGH specifications, loaders, analyzers, and validation
tooling — that make those binaries readable.

## What's here

A family of Ghidra processor modules, one per accelerator ISA, organized
into a single umbrella repository
([**ghidra-accelerators**](https://github.com/xpu-forge/ghidra-accelerators))
with a Gradle multi-project build so you can install one, several, or all
of them in a single command.

### GPUs

| Module                                                                | Vendor / Family                              |
|-----------------------------------------------------------------------|----------------------------------------------|
| [ghidra-cuda](https://github.com/xpu-forge/ghidra-cuda)               | NVIDIA SASS (sm_70 → sm_90)                  |
| [ghidra-amdgpu](https://github.com/xpu-forge/ghidra-amdgpu)           | AMD GCN / RDNA 1–4 / CDNA 1–3                |
| [ghidra-applegpu](https://github.com/xpu-forge/ghidra-applegpu)       | Apple AGX                                    |
| [ghidra-intelgpu](https://github.com/xpu-forge/ghidra-intelgpu)       | Intel Gen9 / Gen12 / Xe-HPG / Xe-HPC / Xe2   |
| [ghidra-mali](https://github.com/xpu-forge/ghidra-mali)               | ARM Mali Midgard / Bifrost / Valhall / Gen5  |
| [ghidra-adreno](https://github.com/xpu-forge/ghidra-adreno)           | Qualcomm Adreno A3xx → A8xx                  |

### NPUs / ML accelerators

| Module                                                                | Vendor / Family                              |
|-----------------------------------------------------------------------|----------------------------------------------|
| [ghidra-tpu](https://github.com/xpu-forge/ghidra-tpu)                 | Google TPU v4 / v5e / v5p / Trillium (v6)    |
| [ghidra-aie](https://github.com/xpu-forge/ghidra-aie)                 | AMD Versal AI Engine (AIE / AIE-ML / AIE-2)  |
| [ghidra-tenstorrent](https://github.com/xpu-forge/ghidra-tenstorrent) | Tenstorrent Tensix (Grayskull/Wormhole/Blackhole) |
| [ghidra-ane](https://github.com/xpu-forge/ghidra-ane)                 | Apple Neural Engine v1 → v4                  |

### DSPs / MCUs / vector extensions

| Module                                                          | Vendor / Family                               |
|-----------------------------------------------------------------|-----------------------------------------------|
| [ghidra-hexagon](https://github.com/xpu-forge/ghidra-hexagon)   | Qualcomm Hexagon scalar + HVX + HMX           |
| [ghidra-rvv](https://github.com/xpu-forge/ghidra-rvv)           | RISC-V Vector 1.0 + matrix-ext + vendor variants |
| [ghidra-falcon](https://github.com/xpu-forge/ghidra-falcon)     | NVIDIA Falcon (fuc4/fuc5) — embedded MCU in NVIDIA GPUs |

## Philosophy

* **Reference disassembler as the truth oracle.** Every module documents
  how to compare its Ghidra output against an external reference
  (`cuobjdump`, `llvm-objdump`, `envydis`, `iga64`, `ir3_disasm`, …) so
  decode coverage is measurable, not aspirational.
* **Topic-based SLEIGH layout.** Each module splits its instruction
  table into `<isa>_base.sinc` + per-topic includes (`_alu.sinc`,
  `_memory.sinc`, `_flow.sinc`, …) so generation deltas and feature
  additions don't bloat a single monolithic spec.
* **UNVALIDATED is a first-class label.** Where the ISA is proprietary
  and no public oracle exists (TPU, ANE), constructors are explicitly
  tagged so users know what they're looking at.
* **One umbrella, independent submodules.** Each ISA lives in its own
  repo with its own milestone roadmap (`PLAN.md`), test suite, and CI.
  The umbrella exists for convenience, not coupling.

## Status

Most modules are at "Phase 2" — template parity across the collection,
with M1 (first instruction family) landed and validated against the
ISA's reference disassembler where one exists. See each module's
`PLAN.md` for its M1 → M5 roadmap.

## License

All modules MIT licensed.
