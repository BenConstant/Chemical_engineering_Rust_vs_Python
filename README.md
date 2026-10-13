# Chemical Engineering: Rust vs Python

**An open-source laboratory for chemical engineering simulations, numerical methods, optimization, and process-management tools using Python, Rust, and web technologies.**

> **Project status:** Early development / architecture and roadmap. The modules below describe planned work; performance results will be published only after implementations and reproducible benchmarks are available.

## Overview

Chemical engineering problems range from mass and energy balances to complex thermodynamic models, transport phenomena, numerical optimization, and industrial process management.

This repository explores how **Python** and **Rust** can be used independently and together to solve these problems. Its focus is not to claim that one language is universally faster or better, but to investigate **correctness, numerical accuracy, performance, maintainability, and interoperability**.

The project is intended for chemical engineers, students, scientists, data engineers, and developers interested in practical scientific software.

## Objectives

- Implement selected chemical engineering models in both Python and Rust.
- Compare equivalent numerical methods under controlled, reproducible conditions.
- Validate numerical results against analytical solutions, reference data, or established software whenever possible.
- Expose selected Rust routines to Python using **PyO3** and **Maturin**.
- Explore portable Linux Python wheels for use in environments such as **Databricks**.
- Develop interactive tools for web and desktop environments.
- Explore optimization, plant operations, and material/resource management.
- Share readable code, mathematical background, limitations, and reproducible experiments.

## Languages and technologies

| Technology | Intended role |
| --- | --- |
| Python | Reference implementations, scientific workflows, analysis and visualization |
| Rust | Numerical kernels, compiled tools, parallel processing, and reusable libraries |
| PyO3 + Maturin | Rust-to-Python bindings and Python package builds |
| HTML/CSS | Web interface layout and styling |
| JavaScript / TypeScript (optional) | Interactive browser behavior when needed |
| Databricks (planned) | Integration tests and execution of compatible Python wheels in data workflows |

Potential libraries include NumPy, SciPy, Polars, Rayon, and other tools selected for each model. Dependencies will be documented per project rather than mandated repository-wide.

## Repository organization

```text
chemical-engineering-rust-vs-python/
├── 01_fundamentals/
│   ├── dynamic_mass_balance/
│   └── energy_balance/
├── 02_thermodynamics/
│   ├── activity_coefficients/
│   └── pitzer_model/
├── 03_transport_phenomena/
│   ├── heat_transfer/
│   ├── mass_transfer/
│   └── navier_stokes/
├── 04_process_simulation/
│   ├── tank_dynamics/
│   └── reactors/
├── 05_optimization/
│   ├── parameter_estimation/
│   └── process_optimization/
├── 06_plant_management/
│   ├── inventory_management/
│   └── production_planning/
├── 07_applications/
│   ├── web/
│   └── desktop/
├── 08_integrations/
│   ├── python_rust/
│   │   └── chemeng_rs/
│   └── databricks/
├── benchmarks/
├── docs/
├── .github/
│   └── workflows/
├── .gitignore
├── LICENSE
└── README.md
```

> Directories are introduced incrementally. A planned directory does not imply a finished implementation.

## Project conventions

A typical numerical study has separate reference and compiled implementations:

```text
01_fundamentals/dynamic_mass_balance/
├── python/
│   ├── model.py
│   └── main.py
├── rust/
│   ├── Cargo.toml
│   └── src/
│       ├── lib.rs
│       └── main.rs
├── tests/
├── data/
├── results/
└── README.md
```

Each completed project should provide:

1. **Physical model:** equations, assumptions, boundary/initial conditions, units, and scope.
2. **Numerical model:** algorithms, discretization, convergence criteria, and error handling.
3. **Verification:** analytical checks or reference solutions and documented tolerances.
4. **Comparison:** matching inputs, algorithms and stopping criteria where feasible.
5. **Performance:** runtime, allocation/memory measurements where available, hardware and software environment, and repeated trials.
6. **Reproducibility:** commands to run tests, regenerate figures and benchmarks, and interpret results.

## First study: Dynamic mass balance

The initial study will simulate a perfectly mixed tank with constant volume and constant volumetric flow rate. One illustrative model is:

\[
\frac{dC}{dt} = \frac{Q}{V}(C_{in} - C)
\]

where:

- \(C\): tank concentration
- \(C_{in}\): inlet concentration
- \(Q\): volumetric flow rate
- \(V\): tank volume
- \(t\): time

For constant \(Q\), \(V\), and \(C_{in}\), the analytical solution is:

\[
C(t) = C_{in} + (C_0 - C_{in})e^{-Qt/V}.
\]

The study will compare a Python reference implementation, a native Rust implementation, and a PyO3-backed Python interface after the implementations are available. Validation against the analytical solution will precede any performance claim.

## Roadmap

| Phase | Topic | Goal | Status |
| --- | --- | --- | --- |
| 1 | Dynamic mass balance | Establish conventions, verification and first Rust/Python comparison | Planned |
| 2 | Energy balances and ODE solvers | Compare numerical integrators and error control | Planned |
| 3 | Python–Rust interoperability | Build the `chemeng_rs` package with PyO3 and Maturin | Planned |
| 4 | Thermodynamics | Activity coefficients and validated Pitzer implementations | Planned |
| 5 | Transport phenomena | Heat/mass transport, then selected Navier–Stokes benchmarks | Planned |
| 6 | Optimization | Parameter estimation and process optimization | Planned |
| 7 | Plant management | Inventory and production planning examples | Planned |
| 8 | Applications | Web/desktop interfaces and reusable tools | Planned |
| 9 | Databricks | Test Linux wheel installation and execution on compatible compute | Planned |

The Pitzer model and Navier–Stokes solvers will be treated as distinct scientific projects, with explicit model assumptions and validation references. Navier–Stokes work will begin with a narrowly specified benchmark rather than a general-purpose CFD solver.

## Comparing Rust and Python fairly

Benchmark results must state:

- Hardware, operating system, Python version, Rust toolchain and library versions.
- Input size, numerical precision, and workload definition.
- Numerical algorithms and tolerances (including whether equivalent implementations are compared).
- Compilation mode, including Rust `--release` where applicable.
- Number of repetitions and reported spread (not only a single best time).
- Whether Python-to-Rust call overhead and data-conversion costs are included.
- Whether compiled Python dependencies such as NumPy/SciPy are used.
- Validation error relative to a reference solution.

Speedups will be reported only for measured, clearly defined workloads. Performance and scientific accuracy are separate acceptance criteria.

## Python + Rust interoperability

Selected Rust algorithms will be exposed through a shared Python package named **`chemeng_rs`**, built with PyO3 and Maturin. The package is planned to group several numerical modules instead of producing a separate binary extension for every exercise.

The interoperability work will document local development, type annotations where useful, test coverage, data-transfer overhead, packaging, and binary compatibility.

For a mixed Python/Rust package, an intended layout is:

```text
08_integrations/python_rust/chemeng_rs/
├── Cargo.toml
├── pyproject.toml
├── src/
│   ├── lib.rs
│   ├── fundamentals.rs
│   └── thermodynamics.rs
└── python/
    └── chemeng_rs/
        └── __init__.py
```

See the official [Maturin mixed-project documentation](https://github.com/PyO3/maturin/blob/main/guide/src/project_layout.md).

## Databricks integration (planned)

A later stage will build a **Linux-compatible Python wheel** containing the compiled Rust extension and test it on an appropriate Databricks runtime.

Proposed workflow:

1. Build the Rust/PyO3 extension for the destination operating system and CPU architecture.
2. Use Maturin with a suitable Linux wheel compatibility policy (for example, a compatible `manylinux` target).
3. Check Python ABI, dependent native libraries, target architecture and Databricks Runtime compatibility.
4. Install the wheel through a supported Databricks library mechanism, such as workspace files or Unity Catalog volumes.
5. Run numerical verification and representative performance tests in notebooks/jobs.

**Important:** Building on Linux alone does not guarantee compatibility with every Databricks environment. Binary dependencies, access modes and library policies must be checked. The first Databricks execution is a future milestone, not a currently verified capability of this repository.

References: [Maturin](https://www.maturin.rs/), [PyO3](https://pyo3.rs/), and [Databricks library installation](https://docs.databricks.com/aws/en/libraries/cluster-libraries).

## Applications

Future projects may provide:

- **Web:** dashboards for simulation inputs, plots, and results.
- **Desktop:** local engineering calculators and simulation utilities.
- **Optimization:** parameter estimation and constrained process problems.
- **Management:** plant inventory, production planning, and resource allocation examples.

Interfaces are separate from numerical model verification, so visual presentation cannot be mistaken for validated physical results.

## Getting started

This repository is being developed incrementally. Start with the `01_fundamentals/dynamic_mass_balance` project when available; each study will include its own dependencies and run instructions.

Suggested development tools:

- Stable Rust toolchain and Cargo
- Python 3 and a virtual environment
- Git
- Maturin for the mixed Python/Rust package

No repository-wide installation command is advertised until working projects and manifests have been committed.

## Contributions

Suggestions, issue reports, validation references, reproducible benchmarks and contributions are welcome. Please describe the mathematical problem, units, test cases, and expected behavior in proposed changes. A formal contribution guide will be added as the repository matures.

## License

**To be selected before the first public release.** See `LICENSE` once added. Until a license is provided, do not assume permission to reuse or redistribute source code beyond applicable law.

## Disclaimer

This repository is for education, research, and software engineering demonstrations. Models require independent validation before use in industrial design, plant operation, safety-critical control, or other consequential decisions.
