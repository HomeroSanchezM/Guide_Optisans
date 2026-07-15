# Practical Example — Genetic Algorithm

Objective: use OptiSANS to automatically find the best GFP deuteration pattern by optimizing SANS fitness.

## Experimental conditions

The conditions are defined in [conditions.md](conditions.md).

## Procedure

### Step 1 — Run the GA

```bash
optisans run gfp_clean.pdb -p 30 -e 5 -g 30 --seed 42 --d2o 0 --d2o 42 --d2o 100
```

Parameters chosen for this tutorial:
- Population of 30 chromosomes (10 preserved elites)
- 30 generations only for speed
- Explored D2O: 0, 42, and 100% (locked via `--d2o`)
- Seed 42 for reproducibility

### Step 2 — Monitor execution

With `--verbose`, the log displays for each generation:
- the best fitness
- the D2O of the best chromosome
- the AAs deuterated in the best chromosome
- flags preserved by elite/crossover/mutation

### Step 3 — Read results

Files produced in `<pdb_stem>_final_results/`:

- `best_solution_summary.csv`: summary of the best chromosome (deuterated AAs, D2O, fitness, ratio, H/D counts).
- `best_fitness_summary.csv`: history of best fitness per generation, with D2O and number of deuterated AAs.

Display the summary:

```bash
cat gfp_clean_final_results/best_solution_summary.csv
cat gfp_clean_final_results/best_fitness_summary.csv
```

### Step 4 — Genome interpretation

The best chromosome is a list of 18 effective genes (linked AA groups). The ASN+ASP and GLU+GLN pairs share the same gene.

Example reading of the best genome:
- If `ASN+ASP` is `True`, both ASN and ASP residues are deuterated together.
- The optimal D2O is the solvent percentage that maximizes fitness for this pattern.

A high fitness indicates that the combination (deuteration pattern + D2O) produces a SANS curve very far from both references, therefore very informative.

## Expected results

See [expected_results.md](expected_results.md).
