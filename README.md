# Quantum Computing Labs: Qiskit/OpenQASM Implementations

## Lab-1: Pauli X, Y, Z Gates

Construct Pauli gates, trace truth table, and visualize on Bloch Sphere (simulate for |0⟩ input).

**Pauli X:**

```qasm
OPENQASM 2.0;
include "qelib1.inc";
qreg q[1];
creg c[1];
x q[0];
measure q[0] -> c[0];
```

(Replace `x` with `y` for Y-gate; `z` for Z-gate. Truth table: X flips |0⟩↔|1⟩, Y/Z phase-flip |1⟩. Bloch: X rotates y→-y, Y z→-z, Z phase on |1⟩.)

## Lab-2: Hadamard Gate

Construct Hadamard, show states on Bloch Sphere.

```qasm
OPENQASM 2.0;
include "qelib1.inc";
qreg q[1];
creg c[1];
h q[0];
measure q[0] -> c[0];
```

(States: |+⟩ = (|0⟩ + |1⟩)/√2. Bloch: |0⟩ north pole → equator x-axis.)

## Lab-3: Hadamard is Self-Inverse

Apply H twice to return to original state.

```qasm
OPENQASM 2.0;
include "qelib1.inc";
qreg q[1];
creg c[1];
h q[0];
h q[0];
measure q[0] -> c[0];
```

(Truth table: Identity; H† = H.)

## Lab-4: H² = I

Prove square of Hadamard is identity.

```qasm
OPENQASM 2.0;
include "qelib1.inc";
qreg q[1];
creg c[1];
h q[0];
h q[0];
measure q[0] -> c[0];
```

(Truth table: Outputs match input; H² = I.)

## Lab-5: Two S Gates = Z Gate

S² = Z (phase shift).

```qasm

OPENQASM 2.0;
include "qelib1.inc";
qreg q[1];
creg c[1];
s q[0];
s q[0];
measure q[0] -> c[0];
```

(Truth table: |0⟩→|0⟩, |1⟩→-|1⟩ like Z.)

## Lab-6: Rx, Ry, Rz Gates

Construct rotations (θ=π/2), show states/angles on Bloch Sphere.

**Rx:**

```qasm
OPENQASM 2.0;
include "qelib1.inc";
qreg q[1];
creg c[1];
rx(pi/2) q[0];
measure q[0] -> c[0];
```

(Replace `rx` with `ry`/`rz`. Bloch: Rx → y-pole, Ry → -z-pole, Rz → equator y-axis.)

## Lab-7: Swap Gate with 3 CNOTs

Implement SWAP, trace truth table.

```qasm
OPENQASM 2.0;
include "qelib1.inc";
qreg q[2];
creg c[2];
cx q[0], q[1];
cx q[1], q[0];
cx q[0], q[1];
measure q[0] -> c[0];
measure q[1] -> c[1];
```

(Truth table: Swaps qubits; e.g., |01⟩→|10⟩.)

## Lab-8: Classical AND with Toffoli

Build AND using CCNOT, trace truth table.

```qasm
OPENQASM 2.0;
include "qelib1.inc";
qreg q[3];
creg c[1];
ccx q[0], q[1], q[2];
measure q[2] -> c[0];
```

(Inputs: q[0],q[1]; Output: q[2] (ancilla |0⟩). Truth table: 00→0, 01→0, 10→0, 11→1.)

## Lab-9: Half Adder with CCNOT/CNOT

Construct half adder, trace truth table.

```qasm
OPENQASM 2.0;
include "qelib1.inc";
qreg q[4];
creg c[2];
cx q[0], q[2];
cx q[1], q[2];
ccx q[0], q[1], q[3];
easure q[2] -> c[0];
measure q[3] -> c[1];
```

(Inputs: q[0],q[1]; Sum: c[0], Carry: c[1]. Truth table: 00→00, 01→10, 10→10, 11→01.)

## Lab-10: Four Bell States

Construct and measure each Bell state.

**Φ⁺ (|00⟩ + |11⟩)/√2:**

```qasm
OPENQASM 2.0;
include "qelib1.inc";
qreg q[2];
creg c[2];
h q[0];
cx q[0], q[1];
measure q[0] -> c[0];
measure q[1] -> c[1];
```

**Φ⁻ (|00⟩ - |11⟩)/√2:**

```qasm
OPENQASM 2.0;
include "qelib1.inc";
qreg q[2];
creg c[2];
h q[0];
cx q[0], q[1];
z q[1];
measure q[0] -> c[0];
measure q[1] -> c[1];
```

**Ψ⁺ (|01⟩ + |10⟩)/√2:**

```qasm
OPENQASM 2.0;
include "qelib1.inc";
qreg q[2];
creg c[2];
h q[0];
cx q[1], q[0];
measure q[0] -> c[0];
measure q[1] -> c[1];
```

**Ψ⁻ (|01⟩ - |10⟩)/√2:**

```qasm
OPENQASM 2.0;
include "qelib1.inc";
qreg q[2];
creg c[2];
h q[0];
cx q[1], q[0];
z q[0];
measure q[0] -> c[0];
measure q[1] -> c[1];
```

## Lab-11a: RX(θ) = H · RZ(θ) · H

Prove equivalence (θ=π/2), trace truth table.

```qasm
OPENQASM 2.0;
include "qelib1.inc";
qreg q[1];
creg c[1];
h q[0];
rz(pi/2) q[0];
h q[0];
measure q[0] -> c[0];
```

(Truth table matches `rx(pi/2)`.)

## Lab-11b: H = RZ(π) · RX(π/2) · RZ(π)

Prove decomposition, trace truth table.

```qasm

OPENQASM 2.0;
include "qelib1.inc";
qreg q[1];
creg c[1];
rz(pi) q[0];
rx(pi/2) q[0];
rz(pi) q[0];
measure q[0] -> c[0];
```

(Truth table matches `h`.)

## Lab-12: T⁴ = Z Gate

Apply T four times, trace truth table, show on Bloch Sphere.

```qasm
OPENQASM 2.0;
include "qelib1.inc";
qreg q[1];
creg c[1];
t q[0];
t q[0];
t q[0];
t q[0];
measure q[0] -> c[0];
```

(Truth table: Like Z; Bloch: |1⟩ phase to south pole.)

## Lab-13: CNOT Gate

Implement CNOT, trace truth table (equivalent to XOR).

```qasm
OPENQASM 2.0;
include "qelib1.inc";
qreg q[2];
creg c[2];
cx q[0], q[1];
measure q[0] -> c[0];
measure q[1] -> c[1];
```

(Truth table: |00⟩→|00⟩, |01⟩→|01⟩, |10⟩→|11⟩, |11⟩→|10⟩; control unchanged, target XOR.)

## Lab-14: T† = Rz(-π/4)

Prove dagger of T is Rz(-π/4).

```qasm
OPENQASM 2.0;
include "qelib1.inc";
qreg q[1];
creg c[1];
tdg q[0];
measure q[0] -> c[0];
```

(Equivalent to `rz(-pi/4) q[0];`.)

## Lab-15: T† Phase Rotation

T† rotates |1⟩ phase by -π/4; no prob change, only relative phase.

```qasm

OPENQASM 2.0;
include "qelib1.inc";
qreg q[1];
creg c[1];
tdg q[0];
measure q[0] -> c[0];

```

(|1⟩ phase: e^{-iπ/4}; measurement probs: 50/50 for superposition, phase -45° relative.)
