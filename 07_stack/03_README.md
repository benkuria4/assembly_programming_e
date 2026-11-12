# `03.asm` — `call` and `ret` with the 64-bit stack

This program uses the stack implicitly for a function return address. It runs in 64-bit Linux mode and exits with status 0.

## Execution and stack flow

1. `_start` sets `RAX` to `10`. This does not affect `RSP`.
2. `call calculate` pushes the address of `after_call` onto the stack and decreases `RSP` by 8, then transfers control to `calculate`.
3. `calculate` adds 5 to `RAX`, making it `15`. The arithmetic instruction does not change the stack.
4. `ret` pops the saved address into the instruction pointer and increases `RSP` by 8, so execution resumes at `after_call` and the stack pointer is restored.
5. The `nop` at `after_call` changes nothing. The program exits with syscall 60 and status 0.

There are no explicit `push` or `pop` instructions in this example. `call` and `ret` perform the push and pop of the return address automatically. `RAX` remains `15` at `after_call`, as the source comment predicts.
