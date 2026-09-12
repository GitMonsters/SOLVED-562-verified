# 🧩 SOLVED-562: Verified ARC-AGI Solvers

One repo, one number: **562 verified solvers**. Every `solve(grid)` in `solves/`
holds an **exact match** against the official held-out test pair(s) — not
partial credit. Run it yourself:

```bash
python3 verify_all.py
# => Results: 562 passed, 0 failed, 0 skipped
```

| Official split | Solved | Available |
|---|---|---|
| ARC-AGI-1 evaluation | **400** | 400 |
| ARC-AGI-2 evaluation | **120** | 120 |
| ARC-AGI-2 training | 424 | 1000 |
| ARC-AGI-1 training | 28 | 400 |

Coverage is per `(split, task)` — 410 task IDs appear in two official splits,
and their solvers were verified against *both* distinct test pairs.

## History

This repo began with one task, **`abc82100`** — the first static-grid puzzle
solved — then grew to the **13 "impossible" Discord tasks**, then a wider
campaign. What used to be three repos are now archived and merged here:

- `SOLVED---abc82100` → archived
- `13-Impossible-ARC-Tasks-SOLVED` → archived (all 13 merged, re-verified)
- `SOLVED-540-of-540` → this repo, renamed

All 13 of the original "impossible" set are ARC-AGI-2 evaluation tasks,
present in `solves/` and verified.

## Layout

```
solves/{id}/solver.py  562 verified solvers — nothing unverifiable in here
dataset/tasks/{id}.json 1147 tasks, byte-identical to an official ARC release
catalog.json            per-solver metadata: name, split(s), exact-match status
verify_all.py           reproduces the 562
unverifiable/           cannot be verified — labelled, kept for history
arc3/                   SEPARATE interactive ARC-AGI-3 agent — NOT in the count
```

## Generalization check

A held-out test pass alone proves little — a solver hardcoded to one output
would pass. So every solver was also run against the **train** pairs it never
needed to satisfy:

| Check | Result |
|---|---|
| Exact match on held-out test pairs | 562 / 562 |
| Also reproduce every train pair | 559 / 562 |
| Also pass a second split's differing pair | 410 / 562 |

The 3 that miss a train pair (`4acc7107`, `5af49b42`, `b942fd60`) are genuine
algorithms with slightly imperfect induction — the opposite of memorization,
which would trivially pass the pairs it was fitted to. Three solvers that *were*
memorization (lookup tables: `f560132c`, `b1fc8b8e`, and the 
`rearc_package`-importing `2dd70a9a`) were rewritten as real algorithms.

## Honest reading

- **This is a catalog of per-task reference solvers, not an ARC benchmark
  score.** Each solver was written for one specific task after inspecting it —
  not a general system solving unseen tasks. That is why published ARC leaderboard
  scores are single-digit and this says nothing about them.
- **520 split-instances are held-out evaluation sets** (400 + 120); the rest are
  training-set tasks (learnable by design, a lower bar).
- **Old numbers were inconsistent.** "540/540", "514", "422", and "665/665" all
  measured overlapping-but-different subsets. This README uses the one number
  reproduced end-to-end: **562**.
- **118 solvers** reference task IDs in *no* ARC dataset; they are excluded and
  kept under `unverifiable/` (see its README).
- **RE-ARC "125/125 (100%)" does not hold** — 0/12 against the official
  generators on fresh inputs. 537 of the 600 legacy RE-ARC dirs map to no
  official task. Documented in `unverifiable/README.md`.
- **`arc3/` scored 2/183 on real ARC-AGI-3 levels**, not the "20/20" quoted
  elsewhere in this account's history.

## How solvers are made

Pure Python `solve(grid)`, synthesized offline via iterative program generation
(observe training pairs → hypothesize rule → write solver → test → iterate →
verify). No ML and no LLM at inference time.

```python
def solve(grid: list[list[int]]) -> list[list[int]]:
    # deterministic transformation
    ...
```

## Notes

- `catalog.json` names are concise summaries, auto-derived from each solver's
  docstring where available.
- Verification is exact-match on the official JSON (no tolerance, no overlap
  credit).

## Contact

Evan Pieser — epieser@protonmail.com