# Conditions — Genetic Algorithm

## Protein

- Input file: `gfp_clean.pdb`
- All hydrogens must be explicit and protonated.

## GA parameters

| Parameter | Value |
|-----------|--------|
| Population size | `30` (multiple of 3) |
| Elitism | `5` (<= population/3) |
| Generations | `30` (short for the tutorial) |
| D2O variation rate | `5` (not used because D2O is locked) |
| Seed | `42` |
| Explored D2O | `0`, `42`, `100` |
| q_max (A^-1) | `0.3` |
| ratio_threshold | `0.01` |
| gamma | `2` |
| Parallel jobs | `150` |
| Concentration (mg/mL) | `2.5` |

## Commands

```bash
optisans run gfp_clean.pdb -p 30 -e 5 -g 30 --seed 42 --d2o 0 --d2o 42 --d2o 100
```

Useful options for adjustment:
- `-p, --population-size`: population size (multiple of 3)
- `-e, --elitism`: number of elites
- `-g, --generations`: number of generations
- `--d2o-var`: D2O mutation amplitude if D2O is not locked
- `--patience`: early stopping (set to 0 to disable)
- `--config`: INI file for all parameters

## Success criteria

- The `<pdb_stem>_final_results/` folder is created.
- `best_solution_summary.csv` contains at least one line with fitness > 0.
- `best_fitness_summary.csv` contains 31 lines (1 header + 30 generations).
- The best individual's PDB is present in `<pdb_stem>_deuterated_pdbs/`.
