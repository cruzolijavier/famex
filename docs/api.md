# API Reference

Reference documentation generated from the `famex` package docstrings.

## Quick Reference

Most usage only needs `Explorer` (and, for advanced cases, `Geometry`/`PathManager`).
Everything else is imported lazily from `famex` and documented in full further down this page.

```{list-table}
:class: fit-table nowrap-col2
:header-rows: 1

* - Name
  - Category
  - Description
* - `Explorer`
  - Core
  - Main entry point: run minima/TS/path optimizations
* - `Geometry`
  - I/O
  - Structure container returned by `read_geometry`
* - `PathManager`
  - I/O
  - Manages multi-structure trajectory files
* - `FrequencyAnalysis`
  - Analysis
  - Vibrational frequencies and thermodynamic properties
* - `HessianCalculator`
  - Analysis
  - Numerical Hessian construction
* - `create_calculator`, `calculator_registry`
  - Backends
  - Build/look up an ASE calculator for a given backend
* - `get_available_backends`, `is_backend_available`
  - Backends
  - Check which ML potential backends are installed
* - `FAMEXError`, `BackendError`, `DependencyError`
  - Errors
  - Base exception types raised by FAMEX
```

## Top-level package

```{eval-rst}
.. automodule:: famex
   :no-members:
```

## Core

```{eval-rst}
.. automodule:: famex.core.explorer
   :members:
   :show-inheritance:

.. automodule:: famex.core.base_strategy
   :members:
   :show-inheritance:

.. automodule:: famex.core.exceptions
   :members:
   :show-inheritance:
```

## Backends

```{eval-rst}
.. automodule:: famex.backends.registry
   :members:
   :show-inheritance:

.. automodule:: famex.backends.availability
   :members:
   :show-inheritance:
```

## Analysis

```{eval-rst}
.. automodule:: famex.analysis.frequency
   :members:
   :show-inheritance:

.. automodule:: famex.analysis.thermodynamics
   :members:
   :show-inheritance:
```

## I/O

```{eval-rst}
.. automodule:: famex.io.geometry
   :members:
   :show-inheritance:

.. automodule:: famex.io.xyz_io
   :members:
   :show-inheritance:
```

## Constraints and interpolation

```{eval-rst}
.. automodule:: famex.constraints.constraints
   :members:
   :show-inheritance:

.. automodule:: famex.interpolation.strategies
   :members:
   :show-inheritance:
```
