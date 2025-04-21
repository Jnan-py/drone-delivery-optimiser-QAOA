# Drone Delivery Optimization via Hybrid Quantum-Classical TSP Solver

This repository contains a hybrid solver that optimizes a drone delivery route using a Traveling Salesman Problem (TSP) formulation. The implementation leverages both quantum and classical algorithms in a hybrid quantum-classical approach:

- **Quantum Component (QAOA):** Uses the Quantum Approximate Optimization Algorithm (QAOA) to explore the solution space by constructing a parameterized quantum circuit. The QAOA is configured with classical optimizers (e.g., COBYLA or SPSA) to iteratively update the circuit parameters.
- **Classical Component (Simulated Annealing):** Uses simulated annealing to generate an initial solution and refine candidates by probabilistically exploring route configurations.
- **Hybrid Strategy:** An iterative loop alternates between QAOA and simulated annealing (SA) to achieve a better overall solution. This is particularly useful for optimization problems (here, drone delivery routing) that are modeled as a TSP.

## Repository Structure

- **Hybrid Solver Functions:**

  - `tsp_solver(tsp_custom, qp)`: Solves the TSP using QAOA with COBYLA.
  - `tsp_solver_qubo(tsp_custom, qp)`: Solves the QUBO version of the TSP using QAOA with SPSA and records the cost history.
  - `simulated_annealing(dist_matrix, ...)`: Implements classical simulated annealing for route optimization.
  - `hybrid_solver(tsp_custom, qp, dist_matrix, iterations)`: Iteratively refines the solution by alternating between QAOA and SA.
  - `draw_graph(G, path=None)`: Visualizes the TSP graph with the route highlighted.

- **Main Script:**

  - The code sets up several TSP instances (including custom and random graphs), runs the quantum, classical, and hybrid solvers, and visualizes the results (including cost convergence plots and route diagrams).

- **QUBO and Ising Conversions:**

  - The script also shows how to convert the problem to a QUBO and then to an Ising model, and solves it using QAOA with COBYLA and SPSA optimizers.

- **QAOA Circuit Visualization:**
  - At the end, the code attempts to display the QAOA circuit diagram by decomposing the ansatz circuit.

## Installation

1. **Clone the repository:**

   ```bash
   git clone https://github.com/Jnan-py/drone-delivery-optimiser-QAOA.git
   cd drone-delivery-optimiser-QAOA
   ```

2. **Set up a virtual environment (optional but recommended):**

   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install the required packages:**

   ```bash
   pip install -r requirements.txt
   ```

## Usage

The script will:

- Create TSP instances (representing drone delivery scenarios).
- Solve the TSP using pure QAOA, simulated annealing, and a hybrid approach.
- Plot the cost vs. iterations for QAOA and simulated annealing.
- Visualize the graph with the computed routes.

## Viewing the QAOA Circuit Diagram

To view the QAOA circuit, the code uses:

```python
qc = qaoa.ansatz
qc.decompose().draw('mpl')
qc.decompose().decompose().draw('mpl')
```

If you only see composite blocks (purple boxes), you can repeatedly call `decompose()` or transpile the circuit into a standard gate set to reveal the full sequence of elementary gates. For example:

```python
from qiskit import transpile
qc_full = transpile(qc, basis_gates=['u3','cx'], optimization_level=0)
qc_full.draw('mpl')
```

## Customization

- **Optimizers:**  
  You can switch between optimizers (COBYLA, SPSA, ADAM) when creating the QAOA instance to compare performance.

- **Repetitions (`reps`):**  
  Change the number of QAOA layers by modifying `reps` in the QAOA constructor.

- **Hybrid Iterations:**  
  Modify the number of iterations in the `hybrid_solver` function to experiment with different hybrid refinement cycles.

## Acknowledgments

- This project leverages Qiskit Optimization, Qiskit Algorithms, and related tools.
- Thanks to the Qiskit community and the many open-source projects that inspired this implementation.
