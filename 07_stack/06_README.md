# `06.asm` — 32-bit values on the stack

This program demonstrates 32-bit `push` and `pop` operations and last-in, first-out order. It exits with status 0 using Linux's 32-bit syscall interface.

## Execution and stack flow

1. `_start` loads `0x11223344` into `EAX` and `0x1234` into `EBX`; `ESP` does not change.
2. `push eax` decreases `ESP` by 4 and stores `0x11223344` at the top of the stack.
3. `push ebx` decreases `ESP` by another 4 and stores `0x1234` above the first value.
4. `pop ebx` retrieves `0x1234`, then increases `ESP` by 4.
5. `pop eax` retrieves `0x11223344`, then increases `ESP` by 4. `ESP` has returned to its initial position.
6. The program sets `EAX = 1` and `EBX = 0`, then `int 0x80` exits with status 0.

In 32-bit mode each push and pop operates on a 4-byte value. As in the 64-bit example, the last value pushed is the first popped, restoring both original register values before the exit setup overwrites them.

## Run observation

The program prints nothing and exits successfully. Its stack behavior is demonstrated by the register values and pointer changes, not by terminal output.
