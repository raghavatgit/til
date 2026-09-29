# sched-ext: Pluggable eBPF CPU Schedulers

`sched_ext` allows implementing domain-specific CPU scheduling algorithms in unprivileged or privileged user-space using eBPF hooks without modifying kernel source.

## Architecture
- `ops.select_cpu()`: Chooses optimal CPU core for waking task based on cache locality and NUMA node.
- `ops.enqueue()`: Dispatches task to central or per-CPU BPF dispatch queues (`scx_bpf_dispatch`).
- `ops.dispatch()`: Consumes tasks and binds to hardware execution contexts.
- Watchdog timer safety: Kernel automatically revokes scheduler and falls back to default CFS/EEVDF if tasks starve.
