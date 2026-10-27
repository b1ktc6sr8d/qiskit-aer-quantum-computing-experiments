# ⚛️ Quantum Computing Simulation Laboratory

A curated collection of foundational, intermediate, and advanced quantum computing experiments implemented using **Qiskit** and simulated with high statistical precision on the **AerSimulator**.

This repository covers the fundamental progression of quantum computing: from single-qubit state preparation and measurement to two-qubit maximal entanglement, experimental violation of local realism (Bell–CHSH inequality), deterministic quantum speedup (Deutsch–Jozsa algorithm), and hybrid quantum-classical optimization (Variational Quantum Eigensolver).

---

## 📑 Table of Contents

- [Overview & Quick Reference](#-overview--quick-reference)
- [Repository Structure](#-repository-structure)
- [Detailed Experiments Overview](#-detailed-experiments-overview)
  - [1. Qubit Initialization & Measurement](#1--qubit-initialization--measurement)
  - [2. Bell States Generation & Entanglement Verification](#2--bell-states-generation--entanglement-verification)
  - [3. Bell–CHSH Inequality Violation & Quantum Non-Locality](#3--bellchsh-inequality-violation--quantum-non-locality)
  - [4. Deutsch–Jozsa Algorithm (Quantum Advantage)](#4--deutschjozsa-algorithm-quantum-advantage)
  - [5. Variational Quantum Eigensolver (VQE)](#5--variational-quantum-eigensolver-vqe)
- [Prerequisites & Installation](#-prerequisites--installation)
- [Running the Notebooks](#-running-the-notebooks)
- [Quantum Gates & Notation Cheat-Sheet](#-quantum-gates--notation-cheat-sheet)
- [License](#-license--acknowledgments)

---

## 🧭 Overview & Quick Reference

| # | Experiment | Notebook | Qubits | Core Quantum Principles | Key Result / Metric |
|---|---|---|:---:|---|---|
| **01** | **Qubit Initialization & Measurement** | [`qubit_initialization_and_measurement.ipynb`](./qubit_initialization_and_measurement.ipynb) | 1 | Wavefunction collapse, Born rule, Pauli-$X$ gate | Deterministic state preparation ($100\%$ $|0\rangle$, $100\%$ $|1\rangle$) |
| **02** | **Bell States & Entanglement** | [`bell_states_and_verification_of_entanglement.ipynb`](./bell_states_and_verification_of_entanglement.ipynb) | 2 | Maximal entanglement, Superposition, Bell basis | Complete correlation ($\|\Phi^\pm\rangle$) & anti-correlation ($\|\Psi^\pm\rangle$) |
| **03** | **Bell–CHSH Inequality Violation** | [`bell-chsh_inequality.ipynb`](./bell-chsh_inequality.ipynb) | 2 | Quantum non-locality, EPR paradox, Tsirelson bound | $S \approx 2.82 > 2.0$ (Violates classical local realism) |
| **04** | **Deutsch–Jozsa Algorithm** | [`dj_algorithm.ipynb`](./dj_algorithm.ipynb) | 4 ($3+1$) | Quantum parallelism, Phase kickback, Interference | $1$ query deterministic classification ($100\%$ accuracy) |
| **05** | **Variational Quantum Eigensolver (VQE)** | [`vqe.ipynb`](./vqe.ipynb) | 1 | NISQ, Rayleigh–Ritz principle, Hybrid optimization | $E_{\text{VQE}} \approx -1.117949 \approx E_{\text{exact}} = -1.118034$ |

---

## 📂 Repository Structure

```text
.
├── qubit_initialization_and_measurement.ipynb         # Exp 1: Single-qubit state prep & Born rule collapse
├── bell_states_and_verification_of_entanglement.ipynb # Exp 2: Generation of 4 Bell states & joint measurement
├── bell-chsh_inequality.ipynb                         # Exp 3: CHSH test of non-locality and bound violation
├── dj_algorithm.ipynb                                 # Exp 4: Deutsch-Jozsa quantum query advantage
├── vqe.ipynb                                          # Exp 5: Ground-state energy estimation via hybrid VQE
├── .gitignore                                         # Ignores checkpoints, virtual envs, and cache files
├── LICENSE                                            # MIT Open-Source License
└── README.md                                          # Comprehensive project documentation
```

---

## 🔬 Detailed Experiments Overview

### 1. 🟢 Qubit Initialization & Measurement
* **File:** [`qubit_initialization_and_measurement.ipynb`](./qubit_initialization_and_measurement.ipynb)
* **Aim:** Initialize a single qubit in standard computational basis states ($|0\rangle$ and $|1\rangle$) and evaluate the empirical measurement statistics using Qiskit's `AerSimulator`.

#### Theory & Mechanics
In standard quantum circuits, qubits are reset by default to the ground state $|0\rangle = \begin{pmatrix} 1 \\ 0 \end{pmatrix}$. Applying the unitary Pauli-$X$ gate (quantum NOT gate) flips the state vector into the excited state:
$$X = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}, \quad X|0\rangle = |1\rangle = \begin{pmatrix} 0 \\ 1 \end{pmatrix}$$

When a projective measurement $M$ in the computational ($\sigma_z$) basis is performed, the continuous wavefunction deterministically collapses into a discrete classical outcome bit $c \in \{0, 1\}$ according to the Born rule:
$$P(0) = |\langle 0|\psi\rangle|^2, \quad P(1) = |\langle 1|\psi\rangle|^2$$

#### Key Findings
- **$|0\rangle$ state:** Sampled over 1,024 shots yielded $100\%$ counts for bit `'0'`.
- **$|1\rangle$ state ($X$ applied):** Sampled over 1,024 shots yielded $100\%$ counts for bit `'1'`.
- Confirmed deterministic gate transformation and verified baseline measurement fidelity in simulation.

---

### 2. 🔗 Bell States Generation & Entanglement Verification
* **File:** [`bell_states_and_verification_of_entanglement.ipynb`](./bell_states_and_verification_of_entanglement.ipynb)
* **Aim:** Construct quantum circuits using Hadamard ($H$) and Controlled-NOT ($CNOT$) gates to generate all four maximally entangled two-qubit Bell states, verifying entanglement through joint measurement statistics.

#### Theory & Circuit Architecture
Bell states form an orthonormal basis of maximally entangled two-qubit states. Starting from the two-qubit computational basis $|q_0 q_1\rangle$, a Hadamard gate is applied to control qubit $q_0$ creating an equal superposition, followed by an entangling $CNOT$ gate with target $q_1$:

$$\text{Circuit: } |x_0 x_1\rangle \xrightarrow{H(q_0)} \xrightarrow{CX(q_0 \to q_1)} |\text{Bell State}\rangle$$

$$\begin{aligned}
|00\rangle &\longrightarrow |\Phi^+\rangle = \frac{|00\rangle + |11\rangle}{\sqrt{2}} \\
|10\rangle &\longrightarrow |\Phi^-\rangle = \frac{|00\rangle - |11\rangle}{\sqrt{2}} \\
|01\rangle &\longrightarrow |\Psi^+\rangle = \frac{|01\rangle + |10\rangle}{\sqrt{2}} \\
|11\rangle &\longrightarrow |\Psi^-\rangle = \frac{|01\rangle - |10\rangle}{\sqrt{2}}
\end{aligned}$$

#### Key Findings & Observations
Across **1,024 shots** per state:
- **Correlated States ($|\Phi^+\rangle, |\Phi^-\rangle$):** Produced exclusively matching bit strings `'00'` ($\approx 50\%$) and `'11'` ($\approx 50\%$). Outcomes `'01'` and `'10'` never appear.
- **Anti-correlated States ($|\Psi^+\rangle, |\Psi^-\rangle$):** Produced exclusively complementary bit strings `'01'` ($\approx 50\%$) and `'10'` ($\approx 50\%$). Outcomes `'00'` and `'11'` never appear.
- Confirmed instantaneous non-local correlation: measurement of one qubit completely determines the state of the second qubit.

---

### 3. 🌌 Bell–CHSH Inequality Violation & Quantum Non-Locality
* **File:** [`bell-chsh_inequality.ipynb`](./bell-chsh_inequality.ipynb)
* **Aim:** Empirically verify quantum non-locality and reject local hidden-variable theories by demonstrating a violation of the Clauser–Horne–Shimony–Holt (CHSH) inequality ($|S| \le 2$).

#### Theory & Mathematical Formulation
Under classical Local Hidden Variable (LHV) theories (local realism), correlations between spatially separated measurement choices $A, A'$ (Alice) and $B, B'$ (Bob) are strictly bounded:
$$S = E(A, B) + E(A, B') + E(A', B) - E(A', B') \implies |S|_{\text{classical}} \le 2$$

In quantum mechanics, evaluating $S$ on the entangled singlet/Bell state $|\Phi^+\rangle$ using optimal measurement bases:
- Alice bases: $a_1 = 0$, $a_2 = \frac{\pi}{2}$
- Bob bases: $b_1 = \frac{\pi}{4}$, $b_2 = -\frac{\pi}{4}$

yields individual expectation values $E(a_i, b_j) = \frac{1}{\sqrt{2}}$, culminating in the **Tsirelson Bound**:
$$S_{\text{quantum}} = 3\left(\frac{1}{\sqrt{2}}\right) - \left(-\frac{1}{\sqrt{2}}\right) = 2\sqrt{2} \approx 2.8284$$

#### Key Findings & Observations
- **Measured CHSH Parameter:** Sampled over **4,096 shots** per basis combination yielded:
  $$S_{\text{exp}} \approx 2.82 \pm 0.02$$
- **Violation Significance:** The experimental value $S \approx 2.82$ significantly breaches the classical local realist limit of $2.0$, closely reaching Tsirelson's maximum quantum bound ($2.828$).
- An angle sweep $S(\theta)$ explicitly mapped the continuous sinusoidal quantum violation region.

---

### 4. ⚡ Deutsch–Jozsa Algorithm (Quantum Advantage)
* **File:** [`dj_algorithm.ipynb`](./dj_algorithm.ipynb)
* **Aim:** Implement the Deutsch–Jozsa quantum algorithm to classify a black-box boolean function (oracle) $f: \{0,1\}^n \to \{0,1\}$ as either **constant** or **balanced** in a single quantum query.

#### Complexity Comparison
- **Classical Deterministic Complexity:** In the worst case, a classical algorithm must inspect over half the possible input configurations before making a deterministic conclusion:
  $$\text{Classical Queries} = 2^{n-1} + 1 \quad (\text{For } n=3, \, \text{queries} = 5)$$
- **Quantum Complexity:** Exactly **$1$ query** regardless of $n$.

#### Algorithm Mechanism
1. Prepare $n$ input qubits in $|0\rangle^{\otimes n}$ and 1 ancilla qubit in $|1\rangle$.
2. Apply Hadamard transforms to create an equal superposition across all inputs, putting the ancilla into $|-\rangle = \frac{|0\rangle - |1\rangle}{\sqrt{2}}$.
3. Query the oracle $U_f$, which uses **phase kickback** to encode the function evaluation into the relative phase:
   $$|x\rangle|-\rangle \xrightarrow{U_f} (-1)^{f(x)} |x\rangle|-\rangle$$
4. Apply a final layer of Hadamard gates on the input register.
   - If $f(x)$ is **constant**: constructive interference occurs only at $|0\rangle^{\otimes n}$.
   - If $f(x)$ is **balanced**: destructive interference eliminates $|0\rangle^{\otimes n}$.

#### Key Findings & Observations
- **Constant Oracle:** Returned $|000\rangle$ with $100\%$ probability ($1,024/1,024$ shots).
- **Balanced Oracle:** Returned $|111\rangle$ with $100\%$ probability ($1,024/1,024$ shots).
- Deterministic $100\%$ classification accuracy achieved in a single query execution.

---

### 5. 🎛️ Variational Quantum Eigensolver (VQE)
* **File:** [`vqe.ipynb`](./vqe.ipynb)
* **Aim:** Find the ground state eigenvalue of a single-qubit non-trivial Hamiltonian $H = 0.5 X + 1.0 Z$ using a parameterized quantum circuit ansatz and classical optimization.

#### Theory & Variational Principle
VQE is a flagship hybrid quantum-classical algorithm optimized for Noisy Intermediate-Scale Quantum (NISQ) processors. It relies on the Rayleigh–Ritz variational theorem:
$$E(\theta) = \frac{\langle \psi(\theta) | H | \psi(\theta) \rangle}{\langle \psi(\theta) | \psi(\theta) \rangle} \ge E_0$$

- **Hamiltonian:** $H = 0.5 X + 1.0 Z = \begin{pmatrix} 1.0 & 0.5 \\ 0.5 & -1.0 \end{pmatrix}$
- **Exact Analytical Ground State (Diagonalization):**
  $$E_0 = -\sqrt{0.5^2 + 1.0^2} = -\sqrt{1.25} \approx -1.118034$$
- **Ansatz:** Parameterized single-qubit rotation $R_y(\theta) = e^{-i \theta Y / 2}$.
- **Expectation Decomposition:**
  $$\langle H \rangle_\theta = 0.5 \langle X \rangle_\theta + 1.0 \langle Z \rangle_\theta$$
  Where $\langle X \rangle$ is measured by rotating with Hadamard into the computational basis, and $\langle Z \rangle$ is measured directly.

#### Key Findings & Observations
- Sweeping parameter $\theta \in [0, 2\pi]$ successfully traced the full variational energy landscape.
- **Optimal Parameter Found:** $\theta^* \approx 3.6176 \text{ rad}$.
- **VQE Minimum Energy:** $E_{\text{VQE}} \approx -1.117949$.
- **Error:** $|\Delta E| \approx 0.000085$ ($< 0.01\%$), achieving near-exact agreement with the theoretical ground state.

---

## 🛠️ Prerequisites & Installation

### System Requirements
- Python 3.9 or higher
- Jupyter Notebook or VS Code / JupyterLab

### Installation
Clone this repository and install the required dependencies:

```bash
# Clone the repository
git clone https://github.com/<your-username>/<your-repo-name>.git
cd <your-repo-name>

# Create and activate a virtual environment (optional but recommended)
python -m venv venv
# On Windows:
venv\Scripts\activate
# On Linux/macOS:
source venv/bin/activate

# Install required quantum libraries
pip install qiskit qiskit-aer matplotlib numpy
```

---

## 🚀 Running the Notebooks

Launch Jupyter Notebook from your terminal in the workspace directory:

```bash
jupyter notebook
```

Click on any notebook to open and execute the cells sequentially:
- [`qubit_initialization_and_measurement.ipynb`](./qubit_initialization_and_measurement.ipynb)
- [`bell_states_and_verification_of_entanglement.ipynb`](./bell_states_and_verification_of_entanglement.ipynb)
- [`bell-chsh_inequality.ipynb`](./bell-chsh_inequality.ipynb)
- [`dj_algorithm.ipynb`](./dj_algorithm.ipynb)
- [`vqe.ipynb`](./vqe.ipynb)

All notebooks are pre-executed with cell outputs and figures preserved for immediate viewing.

---

## 📖 Quantum Gates & Notation Cheat-Sheet

| Gate | Symbol | Matrix | Action on Computational Basis |
|:---:|:---:|:---:|---|
| **Pauli-$X$** | $X$ | $\begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}$ | Bit-flip: $X\|0\rangle = \|1\rangle$, $X\|1\rangle = \|0\rangle$ |
| **Pauli-$Z$** | $Z$ | $\begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}$ | Phase-flip: $Z\|0\rangle = \|0\rangle$, $Z\|1\rangle = -\|1\rangle$ |
| **Hadamard** | $H$ | $\frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix}$ | Superposition: $H\|0\rangle = \|+\rangle$, $H\|1\rangle = \|-\rangle$ |
| **CNOT** | $CX$ | $\begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 1 \\ 0 & 0 & 1 & 0 \end{pmatrix}$ | Flips target if control is $\|1\rangle$: $CX\|10\rangle = \|11\rangle$ |
| **Rotation-$Y$** | $Ry(\theta)$ | $\begin{pmatrix} \cos\frac{\theta}{2} & -\sin\frac{\theta}{2} \\ \sin\frac{\theta}{2} & \cos\frac{\theta}{2} \end{pmatrix}$ | Continuous rotation around the Bloch sphere $Y$-axis |

---

## 📜 License & Acknowledgments

Distributed under the [MIT License](./LICENSE). All experiments are constructed using the [Qiskit](https://qiskit.org/) open-source quantum computing software development framework provided by IBM Quantum.
