# `02.asm` — A 64-bit stack frame

This program demonstrates a function call and a local stack frame using `RBP` as a stable frame pointer. It exits with status 0.

## Execution and stack flow

1. At `_start`, `call work` pushes the address of the instruction following the call onto the stack. In 64-bit mode the return address occupies 8 bytes, so `RSP` decreases by 8.
2. `work` saves the caller's `RBP` with `push rbp` (another 8-byte decrease), then `mov rbp, rsp` makes `RBP` mark the base of this frame.
3. `sub rsp, 16` reserves 16 bytes for two 8-byte local values. `RSP` moves down by 16; the slots are addressed as `[rbp - 8]` and `[rbp - 16]`.
4. The program writes `100` to `[rbp - 8]` and `200` to `[rbp - 16]`. These memory stores do not further change either stack pointer. It loads them into `RAX` and `RBX`, respectively.
5. `mov rsp, rbp` discards the 16-byte local area by restoring `RSP` to the saved-frame-pointer slot. `pop rbp` restores the caller's `RBP` and raises `RSP` by 8.
6. `ret` pops the saved return address and raises `RSP` by 8 again, returning to `_start` with the caller's stack position restored.
7. `_start` exits using syscall 60 with status 0.

The call's return address sits above the saved `RBP`; the two locals are below the frame pointer. `RBP` stays fixed while `RSP` moves to allocate and release local storage.

## Run observation

The program prints nothing and exits successfully. The local values are loaded into registers but are not printed or otherwise used after the function returns.
