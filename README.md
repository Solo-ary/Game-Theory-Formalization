# Game Theory Formalization

This repository contains a Lean 4 formalization of core results connecting
fixed-point theory and finite game theory. The main development proves the
existence of mixed Nash equilibria for finite games by building the following
formal pipeline:

```text
Scarf's lemma -> Brouwer fixed point theorem -> product Brouwer theorem -> Nash equilibrium existence
```

The repository also includes **BrouwerBench**, a small benchmark dataset for
evaluating whether language models can explain the proof structure of this
formalization from Lean context snippets.

## Repository Layout

```text
.
├── Brouwer/                 # Lean formalization project
│   ├── Gametheory/
│   │   ├── Simplex.lean
│   │   ├── Scarf.lean
│   │   ├── Brouwer.lean
│   │   ├── Brouwer_product.lean
│   │   └── Nash.lean
│   ├── GameTheory.lean
│   ├── lakefile.lean
│   └── lean-toolchain
└── benchmarks/              # BrouwerBench dataset, scripts, and reports
```

## Lean Formalization

The Lean project lives in [`Brouwer/`](Brouwer). It uses Lean 4 and mathlib.

### Main Files

- [`Simplex.lean`](Brouwer/Gametheory/Simplex.lean): defines the standard
  simplex `stdSimplex` over a finite type, including pure strategies and
  supporting lemmas for finite sums, continuity, and simplex-valued maps.
- [`Scarf.lean`](Brouwer/Gametheory/Scarf.lean): develops the combinatorial
  framework used to obtain the Scarf-style lemma underlying the fixed-point
  argument.
- [`Brouwer.lean`](Brouwer/Gametheory/Brouwer.lean): proves Brouwer's fixed
  point theorem on a simplex from the combinatorial development.
- [`Brouwer_product.lean`](Brouwer/Gametheory/Brouwer_product.lean): lifts the
  simplex fixed-point theorem to finite products of simplices.
- [`Nash.lean`](Brouwer/Gametheory/Nash.lean): defines finite games, mixed
  strategies, payoff functions, and mixed Nash equilibria, then proves the
  existence theorem using the product Brouwer theorem.

### Key Definitions and Theorems

- `stdSimplex ℝ α`: the standard simplex over a finite type `α`.
- `Brouwer`: fixed-point theorem for continuous self-maps on a simplex.
- `Brouwer_Product`: fixed-point theorem for finite products of simplices.
- `FinGame`: finite game structure with finite players and finite pure strategy
  sets.
- `mixedS`: mixed strategy profiles for a finite game.
- `mixedNashEquilibrium`: predicate for mixed Nash equilibria.
- `ExistsNashEq`: existence of a mixed Nash equilibrium for finite games.

## Requirements

- Lean 4, as specified by [`Brouwer/lean-toolchain`](Brouwer/lean-toolchain)
- Lake
- mathlib, fetched through Lake

The current Lean toolchain is:

```text
leanprover/lean4:v4.22.0
```

## Checking the Lean Project

From the repository root:

```bash
cd Brouwer
lake exe cache get
lake build
```

Open files under `Brouwer/Gametheory/` in an editor with Lean support to inspect
goals and proof states interactively.

## BrouwerBench

[`benchmarks/`](benchmarks) contains **BrouwerBench**, a context-provided
proof-structure QA benchmark for the formalization pipeline:

```text
Scarf -> Brouwer -> Product Brouwer -> Nash
```

The main dataset is:

```text
benchmarks/data/brouwerbench_v1.jsonl
```

It contains 80 hand-checkable questions covering the role of named Lean objects
in the proof pipeline. Each row includes a Lean-style context excerpt, a
natural-language question, a reference answer, evidence anchors, and a 0-2
manual scoring rubric.

### Validate the Dataset

```bash
make -C benchmarks validate
```

### Run a Local Model

The benchmark runner expects Ollama to be running locally with the requested
model available.

```bash
make -C benchmarks run MODEL=qwen3:8b
```

After creating or updating the corresponding manual score file under
`benchmarks/scores/`, generate a report with:

```bash
make -C benchmarks score MODEL=qwen3:8b
```

See [`benchmarks/README.md`](benchmarks/README.md) for the dataset schema,
scoring rules, reported runs, and paper-oriented artifacts.

## References

- N. V. Ivanov, *Beyond Sperner's Lemma*.
- J. F. Nash, *Non-Cooperative Games*, Annals of Mathematics, 1951.

## License

The Lean formalization under [`Brouwer/`](Brouwer) includes its own
[`LICENSE`](Brouwer/LICENSE). Check that file before reusing or redistributing
the formalization.
