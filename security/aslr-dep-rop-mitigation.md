# Exploit Mitigations: ASLR, DEP/NX, and ROP Chains

## 1. Data Execution Prevention (DEP / W^X)
Enforces hardware memory page protection: memory can be writable or executable, but never both simultaneously (`PAGE_EXECUTE_READWRITE` avoidance). Prevents executing shellcode on the stack or heap.

## 2. Address Space Layout Randomization (ASLR)
Randomizes base addresses of stack, heap, executable image, and loaded libraries on each process launch.
On 64-bit systems, entropy typically exceeds 28-32 bits, rendering hardcoded payload pointers ineffective.

## 3. Return-Oriented Programming (ROP)
Attackers bypass DEP by chaining together tiny instruction sequences ('gadgets') ending in `RET` that already exist in executable code pages, setting up register arguments to call `VirtualProtect` or `mprotect`.
