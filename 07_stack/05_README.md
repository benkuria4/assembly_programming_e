# `05.asm` — 64-bit values on the stack

This program shows that 64-bit pushes and pops preserve full register values and follow last-in, first-out order. It exits with status 0.

## Execution and stack flow

1. `_start` loads `0x1122334455667788` into `RAX` and `0x1234` into `RBX`. These instructions leave `RSP` unchanged.
2. `push rax` decreases `RSP` by 8 and stores the full 64-bit `RAX` value on the stack.
3. `push rbx` decreases `RSP` by another 8 and stores `0x1234` at the new top.
4. `pop rbx` retrieves the top value (`0x1234`) into `RBX` and increases `RSP` by 8.
5. `pop rax` retrieves `0x1122334455667788` into `RAX` and increases `RSP` by 8. `RSP` is back where it was before the pushes.
6. The program sets up syscall 60 with exit status 0 and exits.

The two values return to their original registers because the pops target registers in reverse push order. The 64-bit stack operations use 8-byte slots.

## Run observation

 The program exits successfully. The values are restored to their registers but are not displayed
