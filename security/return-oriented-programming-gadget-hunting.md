# Binary Exploitation: ROP Chain Architecture and Gadget Hunting

## The Premise
When Data Execution Prevention (DEP / W^X) marks the stack non-executable, injected shellcode cannot execute directly. Attackers locate existing machine code snippets ('gadgets') inside loaded binaries ending in `RET` (`0xC3`).

## Gadget Assembly Example
To execute `execve('/bin/sh', NULL, NULL)` on x64 Linux, registers must be populated per System V ABI:
- `RDI` = pointer to `"/bin/sh"`
- `RSI` = 0
- `RDX` = 0
- `RAX` = 59 (`sys_execve`)

Chain layout on stack:
1. `address_of(pop rdi; ret)`
2. `address_of("/bin/sh")`
3. `address_of(pop rsi; ret)`
4. `0x0`
5. `address_of(pop rdx; ret)`
6. `0x0`
7. `address_of(pop rax; ret)`
8. `59`
9. `address_of(syscall)`
