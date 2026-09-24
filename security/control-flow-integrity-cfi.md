# Modern Security: Hardware Control Flow Integrity (CFI)

## Attack Surface
Indirect calls and jumps (`CALL RAX`, `JMP RDX`) and function return pointers (`RET`) can be hijacked by memory corruption exploits.

## Intel CET (Control-flow Enforcement Technology)
1. **Shadow Stack**: Dedicated second stack managed exclusively by hardware and marked non-writable by userspace. CPU compares `RET` address on data stack with shadow stack. Mismatch triggers `#CP` exception.
2. **Indirect Branch Tracking (IBT)**: Every indirect jump target must start with a valid `ENDBR64` instruction. Jumping anywhere else instantly crashes the process.
