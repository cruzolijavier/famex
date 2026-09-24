# FAMEX FAQ and Troubleshooting

Common questions about FAMEX usage, installation, and troubleshooting.

## Table of Contents

1. [Installation and Setup](#installation-and-setup)
2. [Using FAMEX](#using-famex)
3. [Troubleshooting](#troubleshooting)
4. [Performance](#performance)
5. [Getting Help](#getting-help)

## Installation and Setup

### Which backend should I choose?

FAMEX defaults to **UMA** (`uma-s-1p2`) via `fairchem-core>=2.21.0`. For the simplest install with no `e3nn` conflicts, use **AIMNet2** (`pip install torch`) and pass `--backend aimnet2`. See the [backend table](USER_GUIDE.md#backend-guide) in the User Guide.

### Can I install multiple backends?

Some backends conflict (UMA vs MACE). Use separate conda environments — see [Dependency Conflicts](USER_GUIDE.md#dependency-conflicts) in the User Guide.

### What Python version do I need?

Python 3.10+ required. The **PET** backend (`pip install famex[pet]`) requires Python 3.11+.

### Backend not available after installation?

Install backend dependencies. UMA: `pip install famex[uma]` or `pip install "fairchem-core>=2.21.0"`. Other backends: see [README](https://github.com/rlaplaza-lab/famex#readme) and [User Guide](USER_GUIDE.md#backend-guide).

## Using FAMEX

### What's the difference between target and strategy?

See [Core Concepts](USER_GUIDE.md#core-concepts). Target (`minima`, `ts`, `path`) is what you want; strategy (`local`, `interpolate`, `neb`, etc.) is how to get there.

### How do I choose convergence criteria?

See [Convergence](TUTORIALS.md#convergence) in the Tutorials. Quick reference: `--fmax 0.1 --steps 100` (testing), `--fmax 0.05 --steps 1000` (default), `--fmax 0.01 --steps 2000` (high precision).

### What file formats are supported?

All ASE-compatible formats (XYZ, CIF, PDB, VASP, and others supported by ASE I/O).

### How do I specify charge and spin?

CLI: `--default-charge` and `--default-spin`. Python: `Explorer(..., default_charge=0, default_spin=1)`. Values are written to `atoms.info` when missing. Required for consistent UMA/MACE/Orb results on charged or open-shell systems.

### How do I use constraints?

`--constraints` accepts semicolon-separated specs, for example:

```bash
famex minima --strategy local molecule.xyz --constraints "fix 0,1,2"
famex minima --strategy local molecule.xyz --constraints "fix 0,1; harmonic_bond 2,3 k=5.0"
```

Supported types include `fix`, `harmonic_position`, `harmonic_bond`, `harmonic_angle`, and `fixinternals_bond` / `fixinternals_angle` / `fixinternals_dihedral`. See the [global options table](USER_GUIDE.md#global-options) in the User Guide.

## Troubleshooting

### Optimization doesn't converge?

Try:
- Increase steps: `--steps 2000`
- Loosen convergence: `--fmax 0.1`
- Change optimizer: `--local-optimizer bfgs`
- Check input structure quality

### Forces too large or unrealistic energies?

Check:
- Backend compatibility with your elements
- Input structure quality (atoms too close?)
- Charge/spin settings
- System size limits

### CUDA out of memory?

Use CPU (`--device cpu`), reduce system size, or use LBFGS optimizer (`--local-optimizer lbfgs`).

### Transition state validation issues?

- Multiple imaginary frequencies: poor TS guess — try interpolation, growing string, or `rfo` / `sella`
- No imaginary frequencies: structure may be a minimum — verify the TS guess
- Use `--freq` or `calculate_frequencies()`; check `ts_analysis["n_imaginary_frequencies"]`

### UMA and MACE both installed but one fails?

They require incompatible `e3nn` versions. Use separate conda environments (see [Dependency Conflicts](USER_GUIDE.md#dependency-conflicts)).

## Performance

### How do I speed up calculations?

Use GPU (`--device cuda`) when available, or relax `--fmax` / `--steps` while prototyping.

### Which backend is fastest?

Depends on system size, hardware, and task. AIMNet2 is typically fast for small organic molecules; UMA is the default general-purpose MLIP. Profile your workload with [`examples/timing_benchmark.py`](https://github.com/rlaplaza-lab/famex/blob/main/examples/timing_benchmark.py).

## Getting Help

### Where can I get help?

[User Guide](USER_GUIDE.md), [Tutorials](TUTORIALS.md), [examples](https://github.com/rlaplaza-lab/famex/tree/main/examples), or [GitHub Issues](https://github.com/rlaplaza-lab/famex/issues).

### How do I report a bug?

Include `famex --version`, Python version, OS, backend and model name, full error message, and a minimal reproducing example.

### Where can I find examples?

See [`examples/README.md`](https://github.com/rlaplaza-lab/famex/tree/main/examples). Quick start: `python examples/cli_demo.py` from the repo root.

---

*Last updated: June 2026*
