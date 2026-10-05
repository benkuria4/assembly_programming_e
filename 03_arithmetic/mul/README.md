# Multiplication and EFLAGS

This section examines the flags produced by the unsigned `mul` instruction in `mul1.asm` and `mul2.asm`. The flag values below are those immediately after `mul`, before the result is stored and before the program's exit sequence.

For unsigned `mul`, **CF** and **OF** are set when the upper half of the product is nonzero, and cleared when the upper half is zero. The other arithmetic flags—**SF**, **ZF**, **AF**, and **PF**—are undefined after `mul`. Undefined means their values cannot be relied on; they should not be described as definitely set or cleared.

## `mul1.asm`

The program multiplies the 8-bit values 25 and 10. For an 8-bit `mul`, the processor multiplies `AL` by the operand and places the 16-bit product in `AX`:

`25 × 10 = 250 = 0x00FA`

The upper half of `AX` is `AH`, which is zero. Therefore:

- **CF=0 (clear):** The upper half of the product, `AH`, is zero; the product fits in the lower half, `AL`.
- **OF=0 (clear):** The upper half, `AH`, is zero, so the product is fully represented in the lower half.
- **SF, ZF, AF, and PF are undefined:** `mul` does not define these arithmetic flags. Their displayed values do not indicate properties of this product.

## `mul2.asm`

The program multiplies the 16-bit values 3000 and 200. For a 16-bit `mul`, the processor multiplies `AX` by the operand and places the 32-bit product in `DX:AX`:

`3000 × 200 = 600000 = 0x000927C0`

The product is split as `DX = 0x0009` and `AX = 0x27C0`. Since the upper half `DX` is nonzero:

- **CF=1 (set):** The upper half of the product, `DX`, is nonzero, so the product does not fit in the lower half, `AX`.
- **OF=1 (set):** The upper half, `DX`, is nonzero.
- **SF, ZF, AF, and PF are undefined:** `mul` does not define these arithmetic flags. Their displayed values do not indicate properties of this product.
