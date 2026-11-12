# `04.asm` — A 32-bit stack frame

This is the 32-bit counterpart to `02.asm`. It uses `EBP` as a frame pointer, `ESP` as the stack pointer, and Linux's 32-bit `int 0x80` exit syscall. It exits with status 0.

## Execution and stack flow

1. At `_start`, `call work` pushes the address of the next instruction onto the stack. A 32-bit return address takes 4 bytes, so `ESP` decreases by 4.
2. `work` saves the caller's `EBP` using `push ebp`, decreasing `ESP` by another 4. `mov ebp, esp` sets the frame pointer to this saved-value location.
3. `sub esp, 8` reserves 8 bytes for two 4-byte local values. `ESP` decreases by 8, while `EBP` remains fixed. The program stores `100` at `[ebp - 4]` and `200` at `[ebp - 8]`.
4. It loads those locals into `EAX` and `EBX`. The loads do not change the stack pointers.
5. `mov esp, ebp` releases the local area. `pop ebp` restores the caller's frame pointer and increases `ESP` by 4.
6. `ret` pops the 4-byte return address, increases `ESP` by 4, and returns to `_start` with the caller's stack position restored.
7. `_start` exits with `EAX = 1` (32-bit `sys_exit`) and `EBX = 0` (exit status).

The return address is above the saved `EBP`, and the locals are at negative offsets from `EBP`. This makes local-variable addresses stable even while `ESP` moves.

## Run observation

The program loads the two local values into registers, but does not print them.
