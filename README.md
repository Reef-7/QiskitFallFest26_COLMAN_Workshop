# Qiskit Fall Fest — Colman Quantum Workshop

A hands-on workshop series for learning quantum computing using IBM Qiskit. The notebooks guide you step by step from installing the tools all the way to running circuits on real IBM quantum hardware or simulators.

---

## Prerequisites

- Python 3.8+
- An [IBM Quantum Platform](https://quantum.ibm.com/) account and API key

Install the required packages:

```bash
pip install qiskit[visualization] qiskit-ibm-runtime matplotlib jupyter pylatexenc
```

For simulator-only notebooks, also install:

```bash
pip install qiskit-aer
```

---

## Notebooks Overview

There are **5 notebooks** in this project (3 main + 2 completed references). They all teach the same core IBM Qiskit "Hello World" workflow, but differ in **execution target** (real hardware vs. simulator) and **how much code is pre-filled**.

### The Core Workflow (shared by all notebooks)

Every notebook follows the same 4-step Qiskit pattern:

| Step | What happens |
|------|--------------|
| 1. Map | Build a quantum circuit (Bell state or GHZ state) and define observables (Pauli operators) |
| 2. Optimize | Transpile the circuit to match the target backend's ISA (gate set + qubit connectivity) |
| 3. Execute | Run the circuit using the `Estimator` primitive and collect results |
| 4. Analyze | Plot expectation values and interpret the results |

---

## Notebook Descriptions

### 1. `IBM_Quantum_hello-world_blank.ipynb`
**Purpose:** Clean reference copy of the official IBM Qiskit "Hello World" guide — runs on **real quantum hardware**.

- All code cells are pre-filled and ready to run (no blanks to fill in).
- Connects to IBM Quantum via `QiskitRuntimeService` and selects the least-busy real QPU.
- Part 1: Creates a 2-qubit **Bell state** and measures 6 Pauli observables (`IZ`, `IX`, `ZI`, `XI`, `ZZ`, `XX`).
- Part 2: Scales up to a **100-qubit GHZ state** using a real IBM QPU with 100+ qubits. Enables **dynamical decoupling** (XY4) for error suppression.
- Results show how expectation values decay with qubit distance due to hardware noise.

> **Requires:** A valid IBM Quantum API key saved via `QiskitRuntimeService.save_account(...)`.

---

### 2. `IBM_Quantum_hello-world_initial_blank.ipynb`
**Purpose:** Workshop starter notebook — runs on **real quantum hardware**. Students fill in their own API key.

- Nearly identical to `IBM_Quantum_hello-world_blank.ipynb` in content and structure.
- The first code cell includes `pip install` commands and a placeholder for the student's API key:
  ```python
  QiskitRuntimeService.save_account(token="Your-API_KEY", overwrite=True)
  ```
- Designed to be the first file a workshop participant opens — they paste their key, run all cells, and observe results on actual quantum hardware.

> **Key difference from `_blank`:** Includes the install + authentication cell at the top so students can get started immediately without any prior setup.

---

### 3. `IBM_Quantum_hello_world_initial_blank_simulator.ipynb`
**Purpose:** Workshop starter notebook — runs entirely on a **local simulator** (no IBM Quantum account needed).

- Same structure and goals as the hardware notebooks, but replaces all real-hardware calls with local simulators:
  - Part 1 (Bell state): Uses `FakeBelemV2` — a fake backend that mimics a real IBM device's noise and topology.
  - Part 2 (100-qubit GHZ state): Uses `AerSimulator(method="matrix_product_state")` — a high-performance simulator capable of handling 100-qubit circuits efficiently.
- For Part 2, the `EstimatorV2` comes from `qiskit_aer.primitives` instead of `qiskit_ibm_runtime`.
- Adds a note in the analysis section explaining that the ideal simulator shows perfect correlations (value = 1 for all qubit distances), unlike real hardware where noise degrades the signal.

> **Key difference from the hardware notebooks:** No IBM Quantum account or API key is needed. Everything runs locally. Great for offline or classroom use.

---

### 4. `‏‏hello-world-quantum-simulator_complete.ipynb` *(completed reference)*
**Purpose:** Fully executed reference version of the simulator notebook, with all cell outputs saved.

- Same code as `IBM_Quantum_hello_world_initial_blank_simulator.ipynb`, but with all outputs (plots, job IDs, warnings) already rendered.
- Useful for verifying what correct results look like before running the code yourself.
- Shows the Bell state correlation plot and the 100-qubit GHZ decay-over-distance plot with actual output values.

---

### 5. `‏‏IBM_Quantum_hello-world-Real_Quantum_Hardware.ipynb` *(completed reference)*
**Purpose:** Fully executed reference version of the real hardware notebook, with all cell outputs saved.

- Same code as `IBM_Quantum_hello-world_blank.ipynb`, with all outputs rendered.
- Includes a real Job ID from an IBM Quantum submission.
- Useful for seeing what real hardware results look like (with noise-induced decay in the GHZ experiment).

---

## Key Differences at a Glance

| Notebook | Target Backend | Pre-filled? | Account Needed? | Outputs Saved? |
|---|---|---|---|---|
| `_blank` | Real QPU | Yes | Yes | No |
| `_initial_blank` | Real QPU | Yes (+ install cell) | Yes | No |
| `_initial_blank_simulator` | Local simulator | Yes (+ install cell) | **No** | No |
| `hello-world-quantum-simulator_complete` | Local simulator | Yes | No | **Yes** |
| `IBM_Quantum_hello-world-Real_Quantum_Hardware` | Real QPU | Yes | Yes | **Yes** |

---

## What You'll Learn

- How to build a **Bell state** and a **GHZ state** using Qiskit's `QuantumCircuit`
- How to define quantum observables using **Pauli operators** (`SparsePauliOp`)
- How to transpile circuits for a specific backend using `generate_preset_pass_manager`
- How to run jobs using the **Estimator primitive** and retrieve expectation values
- The difference between running on **real hardware** vs. a **local simulator**
- How **quantum noise** affects measurement results at scale (the GHZ decay experiment)
- How to apply **error suppression** techniques like dynamical decoupling

---

## Resources
 - [Qiskit Hello World](https://quantum.cloud.ibm.com/docs/en/guides/hello-world)
- [IBM Quantum Platform](https://quantum.ibm.com/)
- [Qiskit Documentation](https://docs.quantum.ibm.com/)
- [Qiskit GitHub](https://github.com/Qiskit/qiskit)
