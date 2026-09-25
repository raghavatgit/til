# Binary Security: Format String Vulnerabilities

## Root Cause
Occurs when user-controlled input is passed directly as the format argument to `printf`:
```c
// VULNERABLE:
printf(user_input);

// SECURE:
printf("%s", user_input);
```

## Exploitation Primitives
1. **Arbitrary Memory Reading (`%x`, `%s`)**:
   Passing `%08x.%08x.%08x` reads successive words off the call stack, leaking stack canaries, ASLR base pointers, and secrets.
2. **Arbitrary Memory Writing (`%n`)**:
   The `%n` specifier writes the count of characters printed so far into a pointer supplied on the stack. Attackers craft field widths to write arbitrary memory addresses.

## Defenses
- Compiler warnings: `-Wformat -Wformat-security` (flags non-literal format strings).
- `FORTIFY_SOURCE=2`: Replaces unsafe format calls with bounds-checked variants.
