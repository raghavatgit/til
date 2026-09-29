# x86-64 4-Level Page Table Translation

## Translation Hierarchy
A 48-bit virtual address is partitioned into:
- Bits 47-39: PML4 (Page Map Level 4 Index) -> 512 entries
- Bits 38-30: PDPT (Page Directory Pointer Table Index) -> 512 entries
- Bits 29-21: PD (Page Directory Index) -> 512 entries
- Bits 20-12: PT (Page Table Index) -> 512 entries
- Bits 11-0: Physical Page Offset (4096 bytes)

## Hardware Translation Lookaside Buffer (TLB)
The CPU caches recent virtual-to-physical mappings in the L1/L2 TLB. Modifying page tables requires explicit TLB shootdown across cores using inter-processor interrupts (IPI) via `INVLPG`.
