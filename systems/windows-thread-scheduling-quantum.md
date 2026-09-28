# Windows NT Kernel Thread Dispatching and Quantum Architecture

## Thread States and Dispatcher Lock

The Windows NT kernel scheduler is a priority-driven, preemptive dispatcher with 32 priority levels divided into two classes:
- **Variable (Dynamic) Levels (0-15)**: Subject to priority boosts (I/O completion, GUI foreground focus, starvation avoidance).
- **Real-Time Levels (16-31)**: Fixed priority, never automatically modified by the kernel.

### Thread Quantum

A quantum is the discrete slice of CPU execution time allocated to a thread before a context switch evaluation occurs.
- On Windows Client (e.g. Windows 10/11), foreground window processes receive 3x quantum multiplier boosts (typically 3 clock ticks).
- On Windows Server, thread quanta are fixed and long (typically 12 clock ticks) to maximize CPU cache locality for background server workloads.

---

## High-Resolution Multimedia Timers

Standard Windows system timer interrupt ticks occur every 15.625 ms (`64 Hz`). For deterministic latency, micro-benchmarking, and competitive gaming input polling:

```c
#include <windows.h>
#include <timeapi.h>

// Request 0.5ms (500us) scheduler tick granularity
timeBeginPeriod(1);
```

### Modern NT Resolution: NtSetTimerResolution

The undocumented NT native syscall provides true fractional millisecond resolution:

```c
typedef NTSTATUS (NTAPI *pfnNtSetTimerResolution)(
    ULONG DesiredResolution,
    BOOLEAN SetResolution,
    PULONG CurrentResolution
);
```

Calling `NtSetTimerResolution(5000, TRUE, &Current)` adjusts the Hardware Timer (HPET or local APIC) to 0.5ms ticks, reducing DPC/APC latency and context switch jitter.
