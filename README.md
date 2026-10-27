# Experiment 01: Building Logical Circuits

## Aim and notation

Aim: to design, implement, simplify and verify combinational logic circuits using Boolean algebra and digital logic gates. All eight functions were simplified, built at gate level, and checked against every input combination (240 rows in total); every final circuit matches its original equation on all rows.

| Symbol | Meaning |
| --- | --- |
| `!A` | NOT A |
| `A & B` | AND |
| `A \| B` | OR |
| `A ^ B` | XOR |
| `A XNOR B` | XNOR, equal to `!(A ^ B)` |

Counting rules used throughout:

- Each gate counts once, whatever its number of inputs (a 3-input AND is one gate).
- One NOT gate per complemented input variable, shared by every term that uses it.
- A NAND or NOR with its inputs tied together is used as an inverter and counts as one NAND or NOR.
- Logic depth is the number of gates on the longest path from an input to the output, NOT gates included.

In every truth table, Expected is computed from the equation exactly as given, and Observed is the output of the final circuit simulated gate by gate. If your lab needs readings taken from Logisim, Proteus or the trainer kit, overwrite the Observed column with those.

## Equation 1: four-input sum of products

F1 simplifies to `(A | C) & (B | D) & !(A & B & C & D)`: 4 gates at depth 2 instead of 9 gates at depth 3.

```text
F1 = (!A & B & C) | (A & B & !D) | (!B & C & D) | (A & !C & D)
```

**Simplification.** F1 = 1 for minterms m(3, 6, 7, 9, 11, 12, 13, 14). Its eight prime implicants form a cycle and none is essential, so no sum of products has fewer than four 3-literal terms: the given SOP is already a minimal SOP. Grouping the zeros instead gives a much smaller product of sums.

| AB \\ CD | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| 00 | 0 | 0 | 1 | 0 |
| 01 | 0 | 0 | 1 | 1 |
| 11 | 1 | 1 | 0 | 1 |
| 10 | 0 | 1 | 1 | 0 |

1. Zeros where A = 0 and C = 0 (m0, m1, m4, m5) give the factor `(A | C)`.
2. Zeros where B = 0 and D = 0 (m0, m2, m8, m10) give `(B | D)`.
3. The single zero m15 (all inputs 1) gives `(!A | !B | !C | !D)`, which by De Morgan is `!(A & B & C & D)`, one 4-input NAND.

```text
F1 = (A | C) & (B | D) & !(A & B & C & D)
```

| Circuit | AND | OR | NOT | NAND | Total | Depth |
| --- | --- | --- | --- | --- | --- | --- |
| Original SOP | 4 | 1 | 4 | 0 | 9 | 3 |
| Simplified POS | 1 | 2 | 0 | 1 | 4 | 2 |

**Truth table (16 combinations).**

| ABCD | Expected | Observed | Status |
| --- | --- | --- | --- |
| 0000 | 0 | 0 | Pass |
| 0001 | 0 | 0 | Pass |
| 0010 | 0 | 0 | Pass |
| 0011 | 1 | 1 | Pass |
| 0100 | 0 | 0 | Pass |
| 0101 | 0 | 0 | Pass |
| 0110 | 1 | 1 | Pass |
| 0111 | 1 | 1 | Pass |
| 1000 | 0 | 0 | Pass |
| 1001 | 1 | 1 | Pass |
| 1010 | 0 | 0 | Pass |
| 1011 | 1 | 1 | Pass |
| 1100 | 1 | 1 | Pass |
| 1101 | 1 | 1 | Pass |
| 1110 | 1 | 1 | Pass |
| 1111 | 0 | 0 | Pass |

## Equation 2: product-of-sums circuit

The given POS is already minimal; the NAND-only version uses 9 NAND gates at depth 3 and matches the original on all 16 rows.

```text
F2 = (A | !B | C) & (!A | B | D) & (B | !C | !D)
```

**Minimality.** F2 = 0 for maxterms M(3, 4, 5, 8, 10, 11). Each sum term covers one pair of zeros: `(A | !B | C)` covers m4, m5; `(!A | B | D)` covers m8, m10; `(B | !C | !D)` covers m3, m11. No pair can be grouped into a larger block, so three 3-literal sums is the minimal POS. The minimal SOP needs four terms, so it is not smaller.

| AB \\ CD | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| 00 | 1 | 1 | 0 | 1 |
| 01 | 0 | 0 | 1 | 1 |
| 11 | 1 | 1 | 1 | 1 |
| 10 | 0 | 1 | 0 | 0 |

**NAND-only implementation.**

1. Group the ones into the minimal SOP (`A & B` and `B & C` are essential): `F2 = (A & B) | (B & C) | (!B & !C & D) | (!A & !B & !D)`.
2. Complement twice and apply De Morgan: `F2 = !( !(A & B) & !(B & C) & !(!B & !C & D) & !(!A & !B & !D) )`.
3. This is a NAND of NANDs: `F2 = NAND( NAND(A, B), NAND(B, C), NAND(!B, !C, D), NAND(!A, !B, !D) )`.
4. Each complemented input comes from a NAND with its inputs tied, for example `!A = NAND(A, A)`.

Converting the given POS directly (each sum becomes a NAND of complemented literals, the final AND becomes a NAND plus a NAND inverter) also needs 9 NAND gates but has depth 4, so the two-level NAND-NAND form above is used.

| Circuit | AND | OR | NOT | NAND | Total | Depth |
| --- | --- | --- | --- | --- | --- | --- |
| Original POS | 1 | 3 | 4 | 0 | 8 | 3 |
| NAND only | 0 | 0 | 0 | 9 | 9 | 3 |

The NAND-only circuit costs one extra gate, the usual price of building from a single gate type.

**Truth table (16 combinations).**

| ABCD | Expected | Observed | Status |
| --- | --- | --- | --- |
| 0000 | 1 | 1 | Pass |
| 0001 | 1 | 1 | Pass |
| 0010 | 1 | 1 | Pass |
| 0011 | 0 | 0 | Pass |
| 0100 | 0 | 0 | Pass |
| 0101 | 0 | 0 | Pass |
| 0110 | 1 | 1 | Pass |
| 0111 | 1 | 1 | Pass |
| 1000 | 0 | 0 | Pass |
| 1001 | 1 | 1 | Pass |
| 1010 | 0 | 0 | Pass |
| 1011 | 0 | 0 | Pass |
| 1100 | 1 | 1 | Pass |
| 1101 | 1 | 1 | Pass |
| 1110 | 1 | 1 | Pass |
| 1111 | 1 | 1 | Pass |

## Equation 3: nested complement logic

De Morgan reduces F3 to `(A | B | C) & !(A & C & D)`, built from 4 NAND/NOR gates at depth 3 instead of 9 basic gates at depth 5.

```text
F3 = !( ((!(A | B)) | (C & D)) & (A | (!(B | C))) )
```

**De Morgan simplification.**

1. Outer complement, `!(X & Y) = !X | !Y`: `F3 = !( !(A | B) | (C & D) ) | !( A | !(B | C) )`.
2. First part: `!( !(A | B) | (C & D) ) = (A | B) & !(C & D) = (A | B) & (!C | !D)`.
3. Second part: `!( A | !(B | C) ) = !A & (B | C)`.
4. So `F3 = ((A | B) & (!C | !D)) | (!A & (B | C))`.
5. Split on A. With A = 1, `F3 = !C | !D`. With A = 0, `F3 = (B & (!C | !D)) | B | C = B | C`.
6. Hence `F3 = (A & (!C | !D)) | (!A & (B | C))`, and the identity `(A & Y) | (!A & X) = (A | X) & (!A | Y)` gives the product of sums below.

```text
F3 = (A | B | C) & (!A | !C | !D) = (A | B | C) & !(A & C & D)
```

The K-map confirms it: the only zeros are m0, m1 (A = B = C = 0) and m11, m15 (A = C = D = 1).

| AB \\ CD | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| 00 | 0 | 0 | 1 | 1 |
| 01 | 1 | 1 | 1 | 1 |
| 11 | 1 | 1 | 0 | 1 |
| 10 | 1 | 1 | 0 | 1 |

**NAND and NOR implementation.**

- `A | B | C` is the complement of `NOR(A, B, C)`, and `!(A & C & D)` is `NAND(A, C, D)`.
- An AND of two signals is a NOR of their complements, so `F3 = NOR( NOR(A, B, C), !NAND(A, C, D) )`.
- `!NAND(A, C, D)` comes from a NAND with its inputs tied. No input inversions are needed.

If ordinary AND and OR gates are also allowed, `(A | B | C) & NAND(A, C, D)` needs only 3 gates at depth 2.

| Circuit | AND | OR | NOT | NAND | NOR | Total | Depth |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Original | 2 | 4 | 3 | 0 | 0 | 9 | 5 |
| NAND/NOR | 0 | 0 | 0 | 2 | 2 | 4 | 3 |

**Truth table (16 combinations).**

| ABCD | Expected | Observed | Status |
| --- | --- | --- | --- |
| 0000 | 0 | 0 | Pass |
| 0001 | 0 | 0 | Pass |
| 0010 | 1 | 1 | Pass |
| 0011 | 1 | 1 | Pass |
| 0100 | 1 | 1 | Pass |
| 0101 | 1 | 1 | Pass |
| 0110 | 1 | 1 | Pass |
| 0111 | 1 | 1 | Pass |
| 1000 | 1 | 1 | Pass |
| 1001 | 1 | 1 | Pass |
| 1010 | 1 | 1 | Pass |
| 1011 | 0 | 0 | Pass |
| 1100 | 1 | 1 | Pass |
| 1101 | 1 | 1 | Pass |
| 1110 | 1 | 1 | Pass |
| 1111 | 0 | 0 | Pass |

## Equation 4: XOR-XNOR combinational circuit

F4 reduces to `(A ^ B) & (B | (C XNOR D))`: 4 gates with XOR/XNOR, against 8 for the best circuit built from AND, OR and NOT only, both at depth 3.

```text
F4 = ((A ^ B) & (C XNOR D)) | (!A & B & (C ^ D))
```

**Simplification.**

1. Expand `A ^ B = (!A & B) | (A & !B)`: `F4 = (!A & B & (C XNOR D)) | (A & !B & (C XNOR D)) | (!A & B & (C ^ D))`.
2. Combine the two `!A & B` terms. Since `(C XNOR D) | (C ^ D) = 1`, they reduce to `!A & B`: `F4 = (!A & B) | (A & !B & (C XNOR D))`.
3. Both terms need `A ^ B`, and when `A ^ B = 1`, B = 1 exactly when A = 0. So `F4 = (A ^ B) & (B | (C XNOR D))`.

| AB \\ CD | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| 00 | 0 | 0 | 0 | 0 |
| 01 | 1 | 1 | 1 | 1 |
| 11 | 0 | 0 | 0 | 0 |
| 10 | 1 | 0 | 1 | 0 |

**Basic-gate version.** Without XOR/XNOR gates, the minimal SOP is `F4 = (!A & B) | (A & !B & C & D) | (A & !B & !C & !D)`. It needs an inverter on every input, a 2-input AND, two 4-input ANDs and a 3-input OR.

| Circuit | AND | OR | NOT | XOR/XNOR | Total | Depth |
| --- | --- | --- | --- | --- | --- | --- |
| Original, as given | 2 | 1 | 1 | 3 | 7 | 3 |
| Simplified, XOR/XNOR | 1 | 1 | 0 | 2 | 4 | 3 |
| Simplified, basic gates only | 3 | 1 | 4 | 0 | 8 | 3 |

The XOR gate tests A ≠ B and the XNOR gate tests C = D in one gate each; in basic gates those same tests cost four inverters and two 4-input ANDs.

**Truth table (16 combinations).**

| ABCD | Expected | Observed | Status |
| --- | --- | --- | --- |
| 0000 | 0 | 0 | Pass |
| 0001 | 0 | 0 | Pass |
| 0010 | 0 | 0 | Pass |
| 0011 | 0 | 0 | Pass |
| 0100 | 1 | 1 | Pass |
| 0101 | 1 | 1 | Pass |
| 0110 | 1 | 1 | Pass |
| 0111 | 1 | 1 | Pass |
| 1000 | 1 | 1 | Pass |
| 1001 | 0 | 0 | Pass |
| 1010 | 0 | 0 | Pass |
| 1011 | 1 | 1 | Pass |
| 1100 | 0 | 0 | Pass |
| 1101 | 0 | 0 | Pass |
| 1110 | 0 | 0 | Pass |
| 1111 | 0 | 0 | Pass |

## Equation 5: five-input logic function

F5 is already minimal: all four product terms are essential prime implicants, so the circuit stays at 9 gates and depth 3.

```text
F5 = (A & B & !C) | (!A & D & E) | (B & C & !E) | (!B & C & D)
```

**Minimality check.** F5 = 1 for 15 of the 32 combinations: m(3, 6, 7, 11, 12, 14, 15, 22, 23, 24, 25, 26, 27, 28, 30). Quine-McCluskey finds 8 prime implicants. Four of them are essential, because each is the only prime implicant that covers one particular minterm:

| Term | Minterm only it covers (ABCDE) |
| --- | --- |
| `A & B & !C` | m25 (11001) |
| `!A & D & E` | m3 (00011) |
| `B & C & !E` | m12 (01100) |
| `!B & C & D` | m23 (10111) |

These four terms already cover all 15 ones, so the given expression is the unique minimal SOP. The minimal POS needs 5 sum terms, and factoring (for example `B & ((A & !C) | (C & !E))`) only adds gates.

| Circuit | AND | OR | NOT | Total | Depth |
| --- | --- | --- | --- | --- | --- |
| Given = minimal SOP | 4 | 1 | 4 | 9 | 3 |

**Truth table (32 combinations).**

| ABCDE | Expected | Observed | Status |
| --- | --- | --- | --- |
| 00000 | 0 | 0 | Pass |
| 00001 | 0 | 0 | Pass |
| 00010 | 0 | 0 | Pass |
| 00011 | 1 | 1 | Pass |
| 00100 | 0 | 0 | Pass |
| 00101 | 0 | 0 | Pass |
| 00110 | 1 | 1 | Pass |
| 00111 | 1 | 1 | Pass |
| 01000 | 0 | 0 | Pass |
| 01001 | 0 | 0 | Pass |
| 01010 | 0 | 0 | Pass |
| 01011 | 1 | 1 | Pass |
| 01100 | 1 | 1 | Pass |
| 01101 | 0 | 0 | Pass |
| 01110 | 1 | 1 | Pass |
| 01111 | 1 | 1 | Pass |
| 10000 | 0 | 0 | Pass |
| 10001 | 0 | 0 | Pass |
| 10010 | 0 | 0 | Pass |
| 10011 | 0 | 0 | Pass |
| 10100 | 0 | 0 | Pass |
| 10101 | 0 | 0 | Pass |
| 10110 | 1 | 1 | Pass |
| 10111 | 1 | 1 | Pass |
| 11000 | 1 | 1 | Pass |
| 11001 | 1 | 1 | Pass |
| 11010 | 1 | 1 | Pass |
| 11011 | 1 | 1 | Pass |
| 11100 | 1 | 1 | Pass |
| 11101 | 0 | 0 | Pass |
| 11110 | 1 | 1 | Pass |
| 11111 | 0 | 0 | Pass |

## Equation 6: six-input mixed logic

The given multi-level form is already smaller than any two-level form; replacing `OR + NOT` with a single NOR gives 9 gates at depth 3, down from 10 gates at depth 4.

```text
F6 = ((A | B) & !(C | D)) | ((C ^ E) & (D | G)) | (!A & B & E)
```

**Analysis.**

- F6 = 1 for 37 of the 64 combinations.
- The minimal SOP has 7 product terms (21 literals) and the minimal POS has 6 sum terms (21 literals). The given form uses 11 literals, so flattening it to two levels would make the circuit larger.
- No term can be removed. With C = 1 and E = 1 the first two terms are both 0, so `!A & B & E` alone produces the ones at A = 0, B = 1, C = 1, E = 1.
- `!(C | D)` is exactly one NOR gate, which removes an inverter and one level from the longest path.

```text
F6 = ((A | B) & NOR(C, D)) | ((C ^ E) & (D | G)) | (!A & B & E)
```

| Circuit | AND | OR | NOT | XOR | NOR | Total | Depth |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Original | 3 | 4 | 2 | 1 | 0 | 10 | 4 |
| Optimised | 3 | 3 | 1 | 1 | 1 | 9 | 3 |

**Truth table (64 combinations).**

| ABCDEG | Expected | Observed | Status |
| --- | --- | --- | --- |
| 000000 | 0 | 0 | Pass |
| 000001 | 0 | 0 | Pass |
| 000010 | 0 | 0 | Pass |
| 000011 | 1 | 1 | Pass |
| 000100 | 0 | 0 | Pass |
| 000101 | 0 | 0 | Pass |
| 000110 | 1 | 1 | Pass |
| 000111 | 1 | 1 | Pass |
| 001000 | 0 | 0 | Pass |
| 001001 | 1 | 1 | Pass |
| 001010 | 0 | 0 | Pass |
| 001011 | 0 | 0 | Pass |
| 001100 | 1 | 1 | Pass |
| 001101 | 1 | 1 | Pass |
| 001110 | 0 | 0 | Pass |
| 001111 | 0 | 0 | Pass |
| 010000 | 1 | 1 | Pass |
| 010001 | 1 | 1 | Pass |
| 010010 | 1 | 1 | Pass |
| 010011 | 1 | 1 | Pass |
| 010100 | 0 | 0 | Pass |
| 010101 | 0 | 0 | Pass |
| 010110 | 1 | 1 | Pass |
| 010111 | 1 | 1 | Pass |
| 011000 | 0 | 0 | Pass |
| 011001 | 1 | 1 | Pass |
| 011010 | 1 | 1 | Pass |
| 011011 | 1 | 1 | Pass |
| 011100 | 1 | 1 | Pass |
| 011101 | 1 | 1 | Pass |
| 011110 | 1 | 1 | Pass |
| 011111 | 1 | 1 | Pass |
| 100000 | 1 | 1 | Pass |
| 100001 | 1 | 1 | Pass |
| 100010 | 1 | 1 | Pass |
| 100011 | 1 | 1 | Pass |
| 100100 | 0 | 0 | Pass |
| 100101 | 0 | 0 | Pass |
| 100110 | 1 | 1 | Pass |
| 100111 | 1 | 1 | Pass |
| 101000 | 0 | 0 | Pass |
| 101001 | 1 | 1 | Pass |
| 101010 | 0 | 0 | Pass |
| 101011 | 0 | 0 | Pass |
| 101100 | 1 | 1 | Pass |
| 101101 | 1 | 1 | Pass |
| 101110 | 0 | 0 | Pass |
| 101111 | 0 | 0 | Pass |
| 110000 | 1 | 1 | Pass |
| 110001 | 1 | 1 | Pass |
| 110010 | 1 | 1 | Pass |
| 110011 | 1 | 1 | Pass |
| 110100 | 0 | 0 | Pass |
| 110101 | 0 | 0 | Pass |
| 110110 | 1 | 1 | Pass |
| 110111 | 1 | 1 | Pass |
| 111000 | 0 | 0 | Pass |
| 111001 | 1 | 1 | Pass |
| 111010 | 0 | 0 | Pass |
| 111011 | 0 | 0 | Pass |
| 111100 | 1 | 1 | Pass |
| 111101 | 1 | 1 | Pass |
| 111110 | 0 | 0 | Pass |
| 111111 | 0 | 0 | Pass |

## Equation 7: NAND-NOR hybrid logic

F7 simplifies to `!C & !(A & B) & (B | !D)`, which is one 3-input NOR: 5 NAND/NOR gates at depth 3 instead of 9 basic gates at depth 4.

```text
F7 = (!(A & B) & !(C | D)) | (!(A | C) & B & D)
```

**Simplification.**

1. De Morgan on the NOR parts: `!(C | D) = !C & !D` and `!(A | C) = !A & !C`.
2. Factor out `!C`: `F7 = !C & ( (!(A & B) & !D) | (!A & B & D) )`.
3. Since `B & !(A & B) = B & (!A | !B) = !A & B`, the bracket becomes `!(A & B) & (!D | (D & B))`.
4. Absorption, `!D | (D & B) = !D | B`, gives `F7 = !C & !(A & B) & (B | !D)`.
5. De Morgan on the whole product: `F7 = !( C | (A & B) | (!B & D) ) = NOR( C, A & B, !B & D )`.

| AB \\ CD | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| 00 | 1 | 0 | 0 | 0 |
| 01 | 1 | 1 | 0 | 0 |
| 11 | 0 | 0 | 0 | 0 |
| 10 | 1 | 0 | 0 | 0 |

**NAND/NOR mapping.**

- `A & B` is `NAND(A, B)` followed by a NAND with its inputs tied.
- `!B & D` is `NOR(B, !D)`, with `!D` from `NOR(D, D)`. This is the only input inversion.
- A 3-input NOR combines `C`, `A & B` and `!B & D`.

| Circuit | AND | OR | NOT | NAND | NOR | Total | Depth |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Original | 3 | 3 | 3 | 0 | 0 | 9 | 4 |
| NAND/NOR | 0 | 0 | 0 | 2 | 3 | 5 | 3 |

**Truth table (16 combinations).**

| ABCD | Expected | Observed | Status |
| --- | --- | --- | --- |
| 0000 | 1 | 1 | Pass |
| 0001 | 0 | 0 | Pass |
| 0010 | 0 | 0 | Pass |
| 0011 | 0 | 0 | Pass |
| 0100 | 1 | 1 | Pass |
| 0101 | 1 | 1 | Pass |
| 0110 | 0 | 0 | Pass |
| 0111 | 0 | 0 | Pass |
| 1000 | 1 | 1 | Pass |
| 1001 | 0 | 0 | Pass |
| 1010 | 0 | 0 | Pass |
| 1011 | 0 | 0 | Pass |
| 1100 | 0 | 0 | Pass |
| 1101 | 0 | 0 | Pass |
| 1110 | 0 | 0 | Pass |
| 1111 | 0 | 0 | Pass |

## Equation 8: six-input advanced combinational logic

The three terms of F8 are non-overlapping cases of A and B, so the given form is already near minimal; using NOR gates for the two complemented ORs gives 8 gates at depth 3 instead of 10 at depth 4.

```text
F8 = ((A ^ B) & (C XNOR D)) | (!(A | B) & C & E) | (A & B & !(D | F))
```

**Analysis.**

- The terms split on A and B: when A ≠ B, `F8 = C XNOR D`; when A = B = 0, `F8 = C & E`; when A = B = 1, `F8 = !D & !F`.
- No two terms are ever 1 for the same A, B, so none can absorb another.
- F8 = 1 for 24 of the 64 combinations. The minimal SOP has 6 product terms of 4 literals each and the minimal POS has 7 sum terms, both larger than the given form.
- `!(A | B)` and `!(D | F)` each become one NOR gate, so two OR + NOT pairs shrink to two gates and the longest path loses a level.

```text
F8 = ((A ^ B) & (C XNOR D)) | (NOR(A, B) & C & E) | (A & B & NOR(D, F))
```

| Circuit | AND | OR | NOT | XOR/XNOR | NOR | Total | Depth |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Original | 3 | 3 | 2 | 2 | 0 | 10 | 4 |
| Optimised | 3 | 1 | 0 | 2 | 2 | 8 | 3 |

**Truth table (64 combinations).**

| ABCDEF | Expected | Observed | Status |
| --- | --- | --- | --- |
| 000000 | 0 | 0 | Pass |
| 000001 | 0 | 0 | Pass |
| 000010 | 0 | 0 | Pass |
| 000011 | 0 | 0 | Pass |
| 000100 | 0 | 0 | Pass |
| 000101 | 0 | 0 | Pass |
| 000110 | 0 | 0 | Pass |
| 000111 | 0 | 0 | Pass |
| 001000 | 0 | 0 | Pass |
| 001001 | 0 | 0 | Pass |
| 001010 | 1 | 1 | Pass |
| 001011 | 1 | 1 | Pass |
| 001100 | 0 | 0 | Pass |
| 001101 | 0 | 0 | Pass |
| 001110 | 1 | 1 | Pass |
| 001111 | 1 | 1 | Pass |
| 010000 | 1 | 1 | Pass |
| 010001 | 1 | 1 | Pass |
| 010010 | 1 | 1 | Pass |
| 010011 | 1 | 1 | Pass |
| 010100 | 0 | 0 | Pass |
| 010101 | 0 | 0 | Pass |
| 010110 | 0 | 0 | Pass |
| 010111 | 0 | 0 | Pass |
| 011000 | 0 | 0 | Pass |
| 011001 | 0 | 0 | Pass |
| 011010 | 0 | 0 | Pass |
| 011011 | 0 | 0 | Pass |
| 011100 | 1 | 1 | Pass |
| 011101 | 1 | 1 | Pass |
| 011110 | 1 | 1 | Pass |
| 011111 | 1 | 1 | Pass |
| 100000 | 1 | 1 | Pass |
| 100001 | 1 | 1 | Pass |
| 100010 | 1 | 1 | Pass |
| 100011 | 1 | 1 | Pass |
| 100100 | 0 | 0 | Pass |
| 100101 | 0 | 0 | Pass |
| 100110 | 0 | 0 | Pass |
| 100111 | 0 | 0 | Pass |
| 101000 | 0 | 0 | Pass |
| 101001 | 0 | 0 | Pass |
| 101010 | 0 | 0 | Pass |
| 101011 | 0 | 0 | Pass |
| 101100 | 1 | 1 | Pass |
| 101101 | 1 | 1 | Pass |
| 101110 | 1 | 1 | Pass |
| 101111 | 1 | 1 | Pass |
| 110000 | 1 | 1 | Pass |
| 110001 | 0 | 0 | Pass |
| 110010 | 1 | 1 | Pass |
| 110011 | 0 | 0 | Pass |
| 110100 | 0 | 0 | Pass |
| 110101 | 0 | 0 | Pass |
| 110110 | 0 | 0 | Pass |
| 110111 | 0 | 0 | Pass |
| 111000 | 1 | 1 | Pass |
| 111001 | 0 | 0 | Pass |
| 111010 | 1 | 1 | Pass |
| 111011 | 0 | 0 | Pass |
| 111100 | 0 | 0 | Pass |
| 111101 | 0 | 0 | Pass |
| 111110 | 0 | 0 | Pass |
| 111111 | 0 | 0 | Pass |

## Performance analysis

Across all eight functions the final circuits use 52 gates against 71 for the equations as written (27% fewer), and no final circuit is deeper than 3 levels.

| Eq. | Inputs | Original gates | Original depth | Final gates | Final depth | Final circuit | Rows verified |
| --- | --- | --- | --- | --- | --- | --- | --- |
| F1 | 4 | 9 | 3 | 4 | 2 | POS with one NAND | 16 / 16 |
| F2 | 4 | 8 | 3 | 9 | 3 | NAND only | 16 / 16 |
| F3 | 4 | 9 | 5 | 4 | 3 | NAND and NOR | 16 / 16 |
| F4 | 4 | 7 | 3 | 4 | 3 | XOR, XNOR, AND, OR | 16 / 16 |
| F5 | 5 | 9 | 3 | 9 | 3 | Unchanged, already minimal | 32 / 32 |
| F6 | 6 | 10 | 4 | 9 | 3 | NOR replaces OR + NOT | 64 / 64 |
| F7 | 4 | 9 | 4 | 5 | 3 | NAND and NOR | 16 / 16 |
| F8 | 6 | 10 | 4 | 8 | 3 | NOR replaces OR + NOT | 64 / 64 |
| Total |  | 71 |  | 52 |  |  | 240 / 240 |

The per-gate breakdown (AND, OR, NOT, XOR/XNOR, NAND, NOR) for each circuit is in its own section above. F2 is the only case that grows, by one gate, because the challenge restricts it to NAND gates.

## Expected results: circuit diagrams

Each equation is shown as originally written and as finally built. Input names sit on the gate leads; a NAND or NOR with both inputs on the same signal is being used as an inverter. In F2 (NAND only) and F4 (basic gates) the inverter on B is drawn twice for layout, but on the board one inverter feeds both gates, which is how it was counted.


| Eq. | Original, as typed | Simplified, as typed |
| --- | --- | --- |
| F1 | `(!A & B & C) \| (A & B & !D) \| (!B & C & D) \| (A & !C & D)` | `(A \| C) & (B \| D) & !(A & B & C & D)` |
| F2 | `(A \| !B \| C) & (!A \| B \| D) & (B \| !C \| !D)` | `(A & B) \| (B & C) \| (!B & !C & D) \| (!A & !B & !D)` |
| F3 | `!((!(A \| B) \| (C & D)) & (A \| !(B \| C)))` | `(A \| B \| C) & !(A & C & D)` |
| F4 | `((A ^ B) & !(C ^ D)) \| (!A & B & (C ^ D))` | `(A ^ B) & (B \| !(C ^ D))` |
| F5 | `(A & B & !C) \| (!A & D & E) \| (B & C & !E) \| (!B & C & D)` | same |
| F6 | `((A \| B) & !(C \| D)) \| ((C ^ E) & (D \| G)) \| (!A & B & E)` | same function; swap in the NOR by hand |
| F7 | `(!(A & B) & !(C \| D)) \| (!(A \| C) & B & D)` | `!C & !(A & B) & (B \| !D)` |
| F8 | `((A ^ B) & !(C ^ D)) \| (!(A \| B) & C & E) \| (A & B & !(D \| F))` | same function; swap in the NORs by hand |

## Result

The eight logic circuits were designed, implemented and verified using Boolean algebra and digital logic gates. Every simplified or converted circuit gave the same output as its original equation for all 240 input combinations (16, 32 or 64 per function).

Simplification cut the total from 71 gates to 52 and brought every circuit to a logic depth of 3 or less. The largest savings came from De Morgan's theorems and product-of-sums grouping (F1 and F3 each lost more than half their gates, F7 four of its nine). F5 was already a minimal sum of products, and the NAND-only F2 needed one extra gate, as expected when a single gate type replaces AND, OR and NOT.
