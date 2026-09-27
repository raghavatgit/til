# Intel CET: Hardware Shadow Stacks & Indirect Branch Tracking

## Return-Oriented Programming (ROP) Defeat
In standard x86 architectures, the call stack mixes program local variables and function return addresses. Buffer overflows overwrite return addresses to execute malicious ROP gadget chains.

---

## Intel CET Defenses
1. **Shadow Stack:** A second, hardware-isolated stack managed by CPU microcode (inaccessible to userland `mov` instructions). The CPU pushes return addresses to both stacks during `call`, and compares them during `ret`. If they differ, the CPU raises an immediate Control Protection (`#CP`) exception.
2. **Indirect Branch Tracking (IBT):** Requires indirect jumps/calls to land strictly on valid `ENDBR32` / `ENDBR64` instruction markers.
