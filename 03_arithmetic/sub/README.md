# Subtraction and EFLAGS

This section examines the flags produced by the `sub` instruction in `sub1.asm` and `sub2.asm`. The flag values below are those immediately after each subtraction, before the program's exit sequence. The following `mov` instruction stores the result and does not change EFLAGS.

## `sub1.asm`

The program subtracts the 8-bit value 80 from 50:

`50 - 80 = -30`

The 8-bit result is `0xE2` (`11100010` in binary). Interpreted as a signed 8-bit value, `0xE2` represents −30.

- **CF=1 (set):** As an unsigned subtraction, 50 is less than 80, so the operation requires a borrow.
- **OF=0 (clear):** The signed result, −30, is within the 8-bit signed range of −128 through 127.
- **SF=1 (set):** The most significant bit of `0xE2` is 1, indicating a negative signed result.
- **ZF=0 (clear):** The result is nonzero.
- **AF=0 (clear):** The low nibbles are `2 - 0`; no borrow is needed from bit 4.
- **PF=1 (set):** The low result byte `0xE2` has four set bits, an even number. x86 sets the parity flag for even parity.

## `sub2.asm`

The program subtracts the 16-bit value 2000 from 1000:

`1000 - 2000 = -1000`

The 16-bit result is `0xFC18` (`1111110000011000` in binary). Interpreted as a signed 16-bit value, `0xFC18` represents −1000.

- **CF=1 (set):** As an unsigned subtraction, 1000 is less than 2000, so the operation requires a borrow.
- **OF=0 (clear):** The signed result, −1000, is within the 16-bit signed range of −32768 through 32767.
- **SF=1 (set):** The most significant bit of `0xFC18` is 1, indicating a negative signed result.
- **ZF=0 (clear):** The result is nonzero.
- **AF=0 (clear):** The low nibbles are `8 - 0`; no borrow is needed from bit 4.
- **PF=1 (set):** The parity flag is calculated from the low byte, `0x18`, which has two set bits, an even number.
