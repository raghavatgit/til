# eBPF CO-RE: Compile Once, Run Everywhere

## The Kernel Header Dependency Problem
Historically, BCC (BPF Compiler Collection) required installing heavy `kernel-devel` packages on production nodes and compiling BPF C code using Clang directly on the host machine at runtime.

---

## Architecture of CO-RE
CO-RE enables compiling BPF programs once into portable bytecode:
1. **BTF (BPF Type Format):** A compact, self-describing type system embedded directly into modern Linux kernel binaries (`/sys/kernel/btf/vmlinux`).
2. **Clang Builtins:** Code uses `bpf_core_read()` and `BPF_CORE_READ()` macros to record type access intent rather than hardcoded struct offsets.
3. **Libbpf Loader Relocations:** Libbpf inspects target kernel BTF at program load time, calculates exact field offsets for that running kernel version, and rewrites bytecode instructions in memory before injection.
