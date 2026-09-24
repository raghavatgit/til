# Hardware Security: Rowhammer Vulnerabilities

## Physical Mechanism
Rapidly activating (opening and closing) the same row of DRAM cells causes capacitive cross-talk and electromagnetic charge leakage into neighboring capacitor rows.
If done sufficiently fast before memory refresh cycles, the neighbor row suffers bitflips without ever being directly accessed.

## Exploitation
By placing page tables adjacent to 'hammered' aggressor memory addresses, attackers flip bits in Page Table Entries (PTEs) to map physical kernel memory into user space with full write permissions.

## Defenses
- Target Row Refresh (TRR) hardware logic in DRAM controllers.
- Doubled memory refresh rate (e.g. 32ms instead of 64ms).
