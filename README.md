# EXPERIMENT--01-BUILDING-LOGICAL-CIRCUITS-


# EXPERIMENT-01: BUILDING LOGICAL CIRCUITS

## 1. Aim

To design, implement, and verify digital logic circuits using fundamental logic gates and complex Boolean expressions.

## 2. Objectives

- Understand the operation of AND, OR, NOT, NAND, NOR, XOR, and XNOR gates.
- Implement Boolean expressions using logic gates.
- Construct combinational logic circuits with multiple inputs and outputs.
- Verify circuit outputs using truth tables.
- Simplify Boolean expressions using Boolean algebra and De Morgan's theorems.
- Analyze circuit complexity and identify opportunities for gate-level optimization.

## 3. Required Components and Tools

- Digital logic trainer kit or breadboard
- AND, OR, NOT, NAND, NOR, XOR, and XNOR gate ICs
- Connecting wires and power supply
- LEDs with suitable current-limiting resistors
- Digital logic simulator, such as Logisim Evolution, Proteus, or Multisim
- Truth table and Boolean algebra reference

## 4. Theory

Digital logic circuits operate on binary values:

- **Logic 0:** LOW
- **Logic 1:** HIGH

Boolean algebra represents digital logic operations mathematically.

| Gate | Boolean expression | Function |
|---|---|---|
| AND | \(Y=A\cdot B\) | Output is 1 when all inputs are 1 |
| OR | \(Y=A+B\) | Output is 1 when at least one input is 1 |
| NOT | \(Y=\overline{A}\) | Inverts the input |
| NAND | \(Y=\overline{AB}\) | Complement of AND |
| NOR | \(Y=\overline{A+B}\) | Complement of OR |
| XOR | \(Y=A\oplus B\) | Output is 1 when inputs differ |
| XNOR | \(Y=\overline{A\oplus B}\) | Output is 1 when inputs match |

## 5. Boolean Logic Equations

Implement the following equations using appropriate logic gates. Draw the logic diagram for each equation and verify its output using a truth table.

### Equation 1: Four-Input Mixed Logic Circuit

\[
Y_1=(A+B)(\overline{C}+D)+AB\overline{D}
\]

**Challenge:** Implement the circuit using AND, OR, and NOT gates. Minimize the number of gates without changing the output.

### Equation 2: Five-Input Complemented Logic

\[
Y_2=\overline{(A+B\overline{C})(D+\overline{E})}
\]

**Challenge:** Apply De Morgan's theorem and construct an equivalent implementation using NAND and NOR gates.

### Equation 3: XOR-Based Logic Circuit

\[
Y_3=(A\oplus B)(C+\overline{D})+\overline{A}BC
\]

**Challenge:** Implement the XOR operation and compare the circuit with an equivalent implementation using only AND, OR, and NOT gates.

### Equation 4: Multi-Level Boolean Expression

\[
Y_4=\overline{(A+B)(C+D)}+AB(C+\overline{D})
\]

**Challenge:** Design the circuit using multiple logic levels, derive a simplified expression, and compare the gate count before and after simplification.

### Equation 5: Six-Input Logic Function

\[
Y_5=(A+B+C)(\overline{A}+D)(B+\overline{E}+F)
\]

**Challenge:** Construct the circuit using product-of-sums logic. Verify all \(2^6=64\) input combinations.

### Equation 6: Nested Complement Logic

\[
Y_6=\overline{\overline{(A+B)}\cdot\overline{(C+D)}}
\]

**Challenge:** Simplify the expression using De Morgan's theorem and verify its equivalence to the simplified circuit.

### Equation 7: Mixed NAND, NOR, and XOR Function

\[
Y_7=\overline{AB}+\overline{(C+D)}\oplus(EF)
\]

**Challenge:** Follow standard operator precedence, implement each subexpression independently, and combine the outputs. Use parentheses explicitly in the circuit design to avoid ambiguity.

### Equation 8: Advanced Combinational Logic

\[
Y_8=(A\oplus B)(C\odot D)+\overline{(A+C)}EF
\]

Here, \(\odot\) denotes the XNOR operation.

**Challenge:** Implement the circuit with six inputs, compare a direct gate-level design with a simplified design, and verify all 64 input combinations.

## 6. Procedure

1. Identify the input variables and output variable for each Boolean equation.
2. Determine the required logic gates.
3. Draw the logic circuit using standard digital logic symbols.
4. Simplify the Boolean expression wherever possible.
5. Build the circuit in a simulator or on a digital logic trainer kit.
6. Apply all possible input combinations.
7. Record the observed output for each combination.
8. Compare the observed outputs with the theoretical truth table.
9. Record the number of gates used before and after simplification.

## 7. Verification Table

Use the following format to document experimental results.

| Input combination | Theoretical output | Observed output | Status |
|---|---|---|---|
| 0000 |  |  |  |
| 0001 |  |  |  |
| 0010 |  |  |  |
| 0011 |  |  |  |
| ... |  |  |  |
| Final combination |  |  |  |

For equations with five or six inputs, extend the table to cover all 32 or 64 combinations, respectively.

## 8. Expected Results

- Successfully construct logic circuits from Boolean expressions.
- Verify the outputs against theoretical truth tables.
- Apply Boolean algebra and De Morgan's theorems to simplify circuits.
- Compare alternative implementations in terms of gate count and logic depth.
- Understand the relationship between Boolean expressions and digital hardware.

## 9. Result

The logic circuits are designed and tested, and their outputs are verified against the corresponding Boolean expressions and truth tables.

*Record the actual experimental observations and verification results before finalizing this section.*

## 10. Viva Questions

1. What is the difference between a universal gate and a basic logic gate?
2. Why are NAND and NOR gates called universal gates?
3. State and prove De Morgan's theorems.
4. How can an XOR gate be implemented using basic gates?
5. What is the difference between sum-of-products and product-of-sums representations?
6. How does Boolean simplification reduce hardware complexity?
7. What is the difference between logic depth and gate count?
8. How many rows are required in the truth table for six input variables?
9. Explain the difference between XOR and XNOR operations.
10. Why is it necessary to verify every possible input combination?

## 11. Repository Structure

```text
EXPERIMENT-01-BUILDING-LOGICAL-CIRCUITS/
├── README.md
├── circuit_diagrams/
├── truth_tables/
├── simulation_files/
└── results/
```

- `circuit_diagrams/` — Logic circuit diagrams.
- `truth_tables/` — Theoretical and observed truth tables.
- `simulation_files/` — Simulator project files.
- `results/` — Screenshots, observations, and gate-count comparisons.

## 12. Learning Outcome

After completing this experiment, students will be able to design, implement, simplify, and verify combinational logic circuits using Boolean algebra and digital logic gates.
