# Addition and EFLAGS

This section examines the flags produced by the `add` instruction in `add1.asm` and `add2.asm`. The flag values below are those immediately after each `add`, before the result is stored and before the program's exit sequence. The following `mov` instruction stores the result and does not change EFLAGS.

## `add1.asm`

The program adds the 8-bit values 120 and 10:

`120 + 10 = 130 = 0x82`

The 8-bit result is `0x82` (`10000010` in binary). As an unsigned value, 130 fits in one byte. As a signed 8-bit value, however, `0x82` represents −126, while the mathematical sum of the signed operands is 130.

- **CF=0 (clear):** The unsigned sum, 130, fits in 8 bits; there is no carry out of the most significant bit.
- **OF=1 (set):** Both signed operands are positive, but their mathematical sum, 130, exceeds the signed 8-bit maximum of 127. The stored byte has its sign bit set, indicating signed overflow.
- **SF=1 (set):** The result's most significant bit is 1.
- **ZF=0 (clear):** The result is not zero.
- **AF=1 (set):** The low nibbles add as `8 + A = 12` in hexadecimal, which carries from bit 3 into bit 4.
- **PF=1 (set):** The result byte `0x82` contains two set bits, an even number. x86 sets the parity flag for even parity.

## `add2.asm`

The program adds the 16-bit values 32000 and 500:

`32000 + 500 = 32500 = 0x7EF4`

The 16-bit result is `0x7EF4` (`0111111011110100` in binary). It fits in both the unsigned 16-bit range and the signed 16-bit range.

- **CF=0 (clear):** The unsigned sum, 32500, fits in 16 bits; there is no carry out of the most significant bit.
- **OF=0 (clear):** Both signed operands are positive, and 32500 is below the signed 16-bit maximum of 32767.
- **SF=0 (clear):** The result's most significant bit is 0.
- **ZF=0 (clear):** The result is not zero.
- **AF=0 (clear):** The low nibbles add as `0 + 4`; there is no carry from bit 3 into bit 4.
- **PF=0 (clear):** The parity flag uses the low byte `0xF4`, which contains five set bits—an odd number. x86 sets PF only for even parity.
