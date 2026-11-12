# `01.asm` — Basic 64-bit push and pop

This program demonstrates how `push` and `pop` use the 64-bit stack. It runs in 64-bit Linux mode and exits with status 0.

## Execution and stack flow

1. `_start` loads `100` into `RAX` and `200` into `RBX`. These moves do not change `RSP`.
2. `push rax` subtracts 8 from `RSP` and stores `100` at the new top of the stack.
3. `push rbx` subtracts another 8 and stores `200` above the earlier value. The top is now `200`; beneath it is `100`.
4. `pop rcx` reads the top value (`200`) into `RCX` and adds 8 to `RSP`.
5. `pop rdx` reads the next value (`100`) into `RDX` and adds another 8. `RSP` is back to its value before the two pushes.
6. `nop` has no effect. The program then invokes Linux's 64-bit exit syscall (`RAX = 60`, `RDI = 0`).

The stack is last-in, first-out: the second value pushed is the first one popped. `RAX` and `RBX` are source registers; the values are recovered in reverse order into `RCX` and `RDX`.
