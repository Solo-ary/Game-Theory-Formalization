# Game Theory Formalization in Lean

This repository contains the Lean 4 development and benchmark artifacts for the paper
*Formalizing Scarf, Brouwer, and Nash in Lean*.

The formalization proves a pipeline

```text
Scarf's combinatorial theorem
  -> Brouwer fixed point theorem on the standard simplex
  -> Brouwer fixed point theorem on finite products of simplices
  -> existence of mixed Nash equilibria in finite games
```

The development builds on Mathlib for finite types, finite sets, real analysis,
compactness, continuity, finite-dimensional spaces, and standard simplices. The
main contribution of this repository is the formal proof pipeline connecting the
finite Scarf-style parity argument to the Brouwer and Nash endpoints.

## Paper Version

The Lean version is pinned by [`lean-toolchain`](lean-toolchain), and dependency
versions are recorded in [`lake-manifest.json`](lake-manifest.json).

For the camera-ready paper, cite a stable repository state, preferably a Git tag
or GitHub release such as:

```text
camera-ready-icml2026
```

To obtain the exact commit hash for a finalized artifact, run:

```bash
git rev-parse HEAD
```

For a shorter display version, run:

```bash
git rev-parse --short HEAD
```

Before recording the hash or creating a release, make sure the repository is
clean:

```bash
git status --short
```

If this command prints nothing, the working tree has no uncommitted changes. If
you edit this README after recording a commit hash, the next commit will have a
new hash. For this reason, a stable tag or GitHub release is usually better than
hard-coding a commit hash inside the README itself.

## Reproducing the Lean Build

This is a Lake project. To verify the formalization, run:

```bash
lake exe cache get
lake build
```

The library root is [`Gametheory.lean`](Gametheory.lean), which imports the main
development modules. Open the Lean files in an editor with the Lean language
server running to inspect goals and proof states interactively.

## Main Formal Statements

| Statement | File | Role |
|---|---|---|
| `Scarf` | `Gametheory/Scarf.lean` | Scarf's combinatorial theorem: existence of a colorful room |
| `Brouwer` | `Gametheory/Brouwer.lean` | fixed point theorem on the standard simplex |
| `Brouwer_Product` | `Gametheory/Brouwer_product.lean` | fixed point theorem on finite products of simplices |
| `ExistsNashEq` | `Gametheory/Nash.lean` | existence of mixed Nash equilibria in finite games |

## Repository Structure

| Path | Purpose |
|---|---|
| `Gametheory/Simplex.lean` | supporting lemmas for standard simplices, including pure strategies and coordinate/evaluation facts |
| `Gametheory/Scarf.lean` | Ivanov-style indexed-order Scarf theorem: indexed orders, dominant sets, cells, rooms, doors, colorful and nearly colorful configurations, and the parity endpoint `Scarf` |
| `Gametheory/Brouwer.lean` | derivation of Brouwer's fixed point theorem on the standard simplex from Scarf's theorem using finite grids, dominance estimates, compactness, and continuity |
| `Gametheory/Brouwer_product.lean` | product-of-simplices fixed point theorem via an explicit embedding-projection construction between a product of simplices and one larger simplex |
| `Gametheory/Nash.lean` | finite games, mixed strategy profiles, expected payoff, Nash map, continuity of the Nash map, and the endpoint theorem `ExistsNashEq` |
| `Gametheory.lean` | umbrella import file for the Lake library |
| `benchmarks/` | BrouwerBench proof-structure QA benchmark derived from this formalization |

## Key Definitions and Constructions

- `stdSimplex ℝ α`: the standard simplex over a finite type `α`.
- `IndexedLOrder`: a family of linear orders indexed by colors.
- `isDominant`, `isRoom`, `isDoor`, `isDoorof`: the finite room-door structure used in the Scarf parity proof.
- `TT n l`: the finite grid of the standard simplex used in the Scarf-to-Brouwer argument.
- `Fcolor`: the grid coloring induced by a continuous self-map of the simplex.
- `ProductSimplices`: finite product of standard simplices.
- `embed_from_product`, `project_to_product`: the embedding-projection pair used to reduce product Brouwer to standard-simplex Brouwer.
- `FinGame`: finite strategic-form game data.
- `mixedS`: type of mixed strategy profiles for a finite game.
- `mixed_g`: expected payoff of a mixed profile.
- `mixedNashEquilibrium`: predicate for mixed Nash equilibria.
- `nash_map`: continuous self-map whose fixed points are mixed Nash equilibria.

## Benchmark Artifacts

The proof-structure QA benchmark is under [`benchmarks/`](benchmarks/). It is a
context-provided question-answering benchmark about the formal proof pipeline,
not a Lean proof-synthesis benchmark.

Each prompt includes:

- a section-level Lean-style prelude from `benchmarks/context/`;
- a task-specific excerpt from the JSONL item;
- a natural-language question about the role of named Lean objects in the proof.

To validate the benchmark files, run:

```bash
make -C benchmarks validate
```

To rerun the current `qwen3:8b` evaluation, start Ollama with that model
available and run:

```bash
make -C benchmarks qwen3
```

For detailed benchmark schema, scoring, model-running commands, and reported
v1 artifacts, see [`benchmarks/README.md`](benchmarks/README.md).

## Suggested Release Workflow

A typical camera-ready artifact workflow is:

```bash
# 1. Check the formalization and benchmark metadata
lake exe cache get
lake build
make -C benchmarks validate

# 2. Commit the finalized artifact
git status --short
git add .
git commit -m "Camera-ready artifact"

# 3. Record or tag the exact artifact state
git rev-parse HEAD
git tag camera-ready-icml2026
git push origin main --tags
```

If the paper or README is edited after this step, repeat the build and create a
new commit or tag. The paper should cite the final stable tag or release used for
the submitted artifact.

## References

- L. E. J. Brouwer, "Beweis der Invarianz der Dimensionenzahl", *Mathematische Annalen*, 1911.
- N. V. Ivanov, "Beyond Sperner's Lemma", 2019.
- J. F. Nash, "Equilibrium Points in N-Person Games", *Proceedings of the National Academy of Sciences*, 1950.
- J. F. Nash, "Non-Cooperative Games", *Annals of Mathematics*, 1951.
- H. E. Scarf, "The Computation of Equilibrium Prices: An Exposition", 1982.
- The mathlib Community, "The Lean Mathematical Library", CPP 2020.
