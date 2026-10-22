# Practical Example : Genetic Algorithm

Objective: use OptiSANS to automatically find the best GFP deuteration pattern by optimizing SANS fitness.

## Experimental conditions

The conditions are defined in [conditions.md](conditions.md).

## Procedure

### Step 1 : Run the GA

```bash
optisans run data/gfp.pdb -p 60 -e 10 -g 50 --seed 42 --d2o 0 --d2o 42 --d2o 100
```

Parameters chosen for this tutorial:
- Population of 60 chromosomes (10 preserved elites)
- 50 generations only for speed
- Explored D2O: 0, 42, and 100% (locked via `--d2o`)
- Seed 42 for reproducibility

the log displays for each generation:
- the best fitness
- the D2O of the best chromosomes

### Step 2 : Read results

Files produced in `<pdb_stem>_final_results/`:

- `pdb/`: the best deuterated PDB (`<pdb_stem>_best_fitness.pdb`) and the reference PDBs in `pdb/ref/`.
- `sans_simulation/`: the best simulated SANS curve (`<pdb_stem>_best_fitness.dat`) and the reference curves in `sans_simulation/ref/`.
- `<pdb_stem>_best_fitness.csv`: summary of the best chromosome (deuterated AAs, D2O, fitness, ratio, H/D counts).
- `config.ini`: the effective configuration of the run.

Display the summary:

```bash
cat gfp_final_results/gfp_best_fitness.csv
```

## Expected results

See [expected_results.md](expected_results.md).
