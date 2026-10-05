# Qiskit Fall Fest — Colman Quantum Workshop

A hands-on workshop series for learning quantum computing using IBM Qiskit. The notebooks guide you step by step from installing the tools all the way to running circuits on real IBM quantum hardware or simulators, and into quantum chemistry with VQE.

---

## Prerequisites

- Python 3.8+
- An [IBM Quantum Platform](https://quantum.ibm.com/) account and API key *(only needed for hardware notebooks)*

Install the base packages:

```bash
pip install qiskit[visualization] qiskit-ibm-runtime matplotlib jupyter pylatexenc
```

For simulator-only notebooks, also install:

```bash
pip install qiskit-aer
```

For the VQE / quantum chemistry notebooks, additionally install:

```bash
pip install qiskit-nature pyscf qiskit-algorithms
```

> **Note on PySCF (Windows):** PySCF can be tricky to install on native Windows. Running inside WSL or a Linux/macOS environment is the smoothest option.

---

## Notebooks Overview

The project contains **7 notebooks** across two tracks:

| Track | Goal |
|-------|------|
| **Hello World** | Learn the Qiskit workflow by running a Bell-state / GHZ-state experiment on a simulator or real hardware |
| **VQE Chemistry** | Compute the ground-state energy of H₂ using the Variational Quantum Eigensolver |

---

## Track 1 — Hello World (Bell State & GHZ State)

All five Hello World notebooks teach the same 4-step Qiskit pattern, but differ in **execution target** (real hardware vs. simulator) and **whether outputs are pre-saved**.

### The Core Workflow

| Step | What happens |
|------|--------------|
| 1. Map | Build a quantum circuit (Bell state or GHZ state) and define Pauli observables |
| 2. Optimize | Transpile the circuit to the target backend's ISA |
| 3. Execute | Run the circuit with the `Estimator` primitive |
| 4. Analyze | Plot expectation values and interpret results |

---

### 1. `IBM_Quantum_hello-world_blank.ipynb`
**Purpose:** Clean reference copy of the official IBM Qiskit "Hello World" guide — runs on **real quantum hardware**.

- All code cells are pre-filled and ready to run.
- Connects to IBM Quantum via `QiskitRuntimeService` and selects the least-busy real QPU.
- **Part 1:** Creates a 2-qubit **Bell state** and measures 6 Pauli observables (`IZ`, `IX`, `ZI`, `XI`, `ZZ`, `XX`).
- **Part 2:** Scales up to a **100-qubit GHZ state** on a real IBM QPU. Enables **dynamical decoupling (XY4)** for error suppression.
- Results show how expectation values decay with qubit distance due to hardware noise.

> **Requires:** A valid IBM Quantum API key saved via `QiskitRuntimeService.save_account(...)`.

---

### 2. `IBM_Quantum_hello-world_initial_blank.ipynb`
**Purpose:** Workshop starter notebook for **real quantum hardware**. Students paste in their own API key and run.

- Nearly identical in content to `_blank`.
- The **first code cell** includes `pip install` commands and a placeholder for the student's API key:
  ```python
  QiskitRuntimeService.save_account(token="Your-API_KEY", overwrite=True)
  ```
- Designed as the entry point for workshop participants — paste your key, run all cells, observe results on actual quantum hardware.

> **Key difference from `_blank`:** Adds the install + authentication cell at the top so students can get started without any prior setup.

---

### 3. `IBM_Quantum_hello_world_initial_blank_simulator.ipynb`
**Purpose:** Workshop starter notebook that runs entirely on a **local simulator** — no IBM Quantum account needed.

- Same structure and goals as the hardware notebooks, but uses local simulators:
  - **Part 1 (Bell state):** Uses `FakeBelemV2` — a fake backend that mimics a real IBM device's noise model.
  - **Part 2 (100-qubit GHZ state):** Uses `AerSimulator(method="matrix_product_state")` — handles large circuits efficiently without a queue.
- For Part 2, `EstimatorV2` is imported from `qiskit_aer.primitives` instead of `qiskit_ibm_runtime`.
- Analysis section notes that the ideal simulator returns perfect correlations (value = 1 for all qubit distances), unlike real hardware where noise degrades the signal.

> **Key difference:** No IBM Quantum account needed. Runs fully offline. Great for classroom or restricted-network environments.

---

### 4. `‏‏hello-world-quantum-simulator_complete.ipynb` *(completed reference)*
**Purpose:** Fully executed version of the simulator notebook with all cell outputs saved.

- Same code as `IBM_Quantum_hello_world_initial_blank_simulator.ipynb`, but with all plots, job IDs, and outputs already rendered.
- Useful for previewing expected results before running the notebook yourself.

---

### 5. `‏‏IBM_Quantum_hello-world-Real_Quantum_Hardware.ipynb` *(completed reference)*
**Purpose:** Fully executed version of the real hardware notebook with all cell outputs saved.

- Same code as `IBM_Quantum_hello-world_blank.ipynb`, with all outputs rendered.
- Includes a real Job ID from an IBM Quantum submission.
- Useful for seeing what real hardware results look like, including noise-induced decay in the GHZ experiment.

---

### Hello World — Quick Comparison

| Notebook | Target Backend | Install cell? | Account Needed? | Outputs Saved? |
|---|---|---|---|---|
| `_blank` | Real QPU | No | Yes | No |
| `_initial_blank` | Real QPU | Yes | Yes | No |
| `_initial_blank_simulator` | Local simulator | Yes | **No** | No |
| `hello-world-quantum-simulator_complete` | Local simulator | Yes | No | **Yes** |
| `IBM_Quantum_hello-world-Real_Quantum_Hardware` | Real QPU | No | Yes | **Yes** |

---

## Track 2 — VQE Quantum Chemistry (BasQ Challenge)

These two notebooks were created for the **BasQ Qiskit Fall Fest 2026** (Basque Quantum) by Benjamin Tirado. They introduce a more advanced application: computing molecular ground-state energies using a **hybrid quantum-classical algorithm**.

### The Core Workflow

| Step | What happens |
|------|--------------|
| 1. Define | Describe the molecule (geometry, basis set) using `PySCFDriver` |
| 2. Map | Convert the fermionic Hamiltonian to qubit Pauli operators via **Jordan-Wigner** |
| 3. Build ansatz | Construct a **Hartree-Fock + UCCSD** parameterized circuit |
| 4. Run VQE | Optimize circuit parameters to minimize the expected energy |
| 5. Evaluate | Compare VQE result to the exact classical diagonalization |

---

### 6. `VQESimulator_BasqueQuantum(BasQ)_Challenge1_Blank.ipynb`
**Purpose:** Workshop starter notebook — students work through the VQE workflow hands-on.

- Computes the **ground-state energy of the H₂ molecule** at its equilibrium bond distance (0.735 Å).
- Uses the minimal `sto3g` basis → 2 spatial orbitals → **4 qubits**.
- Applies the **Jordan-Wigner** transformation to convert the fermionic Hamiltonian into 15 Pauli terms.
- Constructs a **UCCSD ansatz** initialized from the **Hartree-Fock** reference state (3 variational parameters).
- Optimizes using the **SLSQP** gradient-based optimizer with the exact `StatevectorEstimator` (noiseless).
- Compares the VQE result to `NumPyMinimumEigensolver` (exact classical diagonalization).
- Results are plotted: Hartree-Fock baseline vs. VQE (UCCSD) vs. exact energy.
- All code cells are present but **outputs are cleared** — intended for students to run and explore.

> **Requires:** `qiskit-nature`, `pyscf`, `qiskit-algorithms`

**Challenge levels (open-ended, described in the notebook):**
- **Beginner:** Compute the potential energy surface (PES) of HeH⁺ as a function of bond distance.
- **Intermediate:** Scale to LiH or BeH₂ and reduce quantum resources (qubit tapering, symmetry exploitation, alternative mappings).
- **Advanced:** Go beyond a fixed ansatz — explore adaptive or hybrid classical-quantum strategies for strongly correlated systems.
- **Hardware extension:** Run on a real IBM Quantum device (e.g. `ibm_basquecountry`) and apply error mitigation.

---

### 7. `VQESimulator_BasqueQuantum(BasQ)_Challenge1_Completed.ipynb`
**Purpose:** Fully executed reference version of the VQE notebook with all outputs saved.

- Identical code to `_Blank`, but all cell outputs are pre-rendered (install logs, molecule parameters, Pauli operator list, circuit diagram, energies, bar chart).
- Shows the expected numerical results:
  - 2 spatial orbitals, 4 qubits, 15 Pauli terms, 3 variational parameters
  - Nuclear repulsion energy: ~0.7200 Ha
- Useful for verifying correct output before running the blank version yourself, or for reference during the hackathon.

---

## What You'll Learn

### Hello World Track
- How to build a **Bell state** and a **GHZ state** using `QuantumCircuit`
- How to define quantum observables with **Pauli operators** (`SparsePauliOp`)
- How to transpile circuits for a specific backend using `generate_preset_pass_manager`
- How to run jobs using the **Estimator primitive** and retrieve expectation values
- The difference between **real hardware** and **local simulators**
- How **quantum noise** affects measurement results at scale
- How to apply **error suppression** (dynamical decoupling)

### VQE Chemistry Track
- How to represent a molecule and compute its Hamiltonian using `PySCFDriver`
- How the **Jordan-Wigner** transformation maps fermionic operators to qubit Pauli strings
- What the **variational principle** is and why it guarantees a lower bound on energy
- How to build a **UCCSD ansatz** starting from a **Hartree-Fock** reference state
- How **VQE** combines a quantum estimator with a classical optimizer
- What **chemical accuracy** (1.6 × 10⁻³ Ha ≈ 1 kcal/mol) means in practice
- How to compare VQE results to exact classical diagonalization

---

## Resources

- [IBM Quantum Platform](https://quantum.ibm.com/)
- [Qiskit Documentation](https://docs.quantum.ibm.com/)
- [Qiskit Nature Documentation](https://qiskit-community.github.io/qiskit-nature/)
- [Ground-State Solvers Tutorial (Qiskit Nature)](https://qiskit-community.github.io/qiskit-nature/tutorials/03_ground_state_solvers.html)
- [Kandala et al., *Hardware-efficient VQE*, Nature 2017](https://www.nature.com/articles/nature23879)
- [Qiskit GitHub](https://github.com/Qiskit/qiskit)
