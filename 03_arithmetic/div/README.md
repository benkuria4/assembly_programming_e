# Division and EFLAGS

This section examines the unsigned `div` instruction in `div1.asm` and `div2.asm`. The results below are the quotient and remainder produced by each division. The arithmetic flags **CF, OF, SF, ZF, AF, and PF are undefined after `div`**. This means the processor does not guarantee their values; any values displayed by GDB must not be interpreted as flags indicating the division result.

## `div1.asm`

The program loads 100 into `AX` and 7 into `BL`. The 8-bit `div bl` instruction divides the 16-bit value in `AX` by `BL`. It places the 8-bit quotient in `AL` and the 8-bit remainder in `AH`:

`100 ÷ 7 = 14 remainder 2`

After division, `AL = 14` and `AH = 2`. This satisfies `100 = (7 × 14) + 2`, with a remainder smaller than the divisor. The quotient fits in `AL`, so the division completes without a divide error.

After `div bl`, **CF, OF, SF, ZF, AF, and PF are all undefined**. Division does not set or clear these flags to describe its quotient or remainder.

## `div2.asm`

The program forms the 32-bit dividend `DX:AX` with `DX = 0` and `AX = 50000`, then loads the 16-bit divisor 300 into `BX`. The 16-bit `div bx` instruction divides `DX:AX` by `BX`. It places the 16-bit quotient in `AX` and the 16-bit remainder in `DX`:

`50000 ÷ 300 = 166 remainder 200`

After division, `AX = 166` and `DX = 200`. This satisfies `50000 = (300 × 166) + 200`, with a remainder smaller than the divisor. The quotient fits in `AX`, so the division completes without a divide error.

After `div bx`, **CF, OF, SF, ZF, AF, and PF are all undefined**. Division does not set or clear these flags to describe its quotient or remainder.

## How to inspect the results and flags

In GDB, break at `_start`, run, then single-step through the setup instructions and the `div` instruction. For `div1.asm`, use three `si` commands; for `div2.asm`, use five. At that point, inspect the result registers (`EAX` and `EDX`) and EFLAGS:

```text
break _start
run
si
si
si
info registers eax edx eflags
```

For `div2.asm`, issue two additional `si` commands before inspecting. Record the quotient and remainder from the result registers. Do not claim that displayed arithmetic flag bits are set or cleared by `div`; their values are undefined by the instruction. Inspect before the exit sequence, whose `xor ebx, ebx` changes flags.
