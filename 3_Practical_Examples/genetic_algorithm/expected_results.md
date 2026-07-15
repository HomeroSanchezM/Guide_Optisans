# Expected Results — Genetic Algorithm

## Generated files

After GA execution, the following folders are created:

- `gfp_clean_deuterated_pdbs/` — one PDB per chromosome and per generation
- `gfp_clean_primus_out/` — simulated SANS curves (.dat) and ref/ folder
- `gfp_clean_final_results/` — best solution and fitness history
  - `best_solution_summary.csv`
  - `best_fitness_summary.csv`

## Reading the best genome

The file `best_solution_summary.csv` contains:

- `generation`, `index`: generation and number of the best chromosome
- `d2o`: optimal D2O percentage
- `deut_count`: number of deuterated AAs (genes set to True)
- `fitness`: final fitness score
- `ratio`: Imax/background ratio
- Columns per AA (18 effective + linked pairs): True/False

Example interpretation:

- If the column `ASN+ASP` is `True`, both ASN and ASP residues are deuterated together.
- If `d2o = 42`, the optimal solvent is 42% D2O.
- If `fitness` increases across generations, the GA converges toward an informative pattern.

## Convergence plot interpretation

`best_fitness_summary.csv` contains the generation-by-generation history:

- `best_fitness`: best fitness at each generation
- `best_d2o`: D2O of the best individual
- `best_deut_count`: number of deuterated AAs

A curve that rises quickly then stabilizes indicates good convergence. If it stagnates, increase the population size or the number of generations.

## Quick troubleshooting

| Problem | Likely cause | Solution |
|----------|---------------|----------|
| Null fitness after 30 generations | population too small or missing references | increase `-p`, check `ref/` |
| No convergence | `--d2o` too restrictive or `--d2o-var` too small | broaden D2O values or increase `--d2o-var` |
| `multiple of 3` error | `population_size` is not a multiple of 3 | use 30, 60, 90, etc. |
| GA is slow | too many parallel jobs or generations too long | reduce `-g` or increase `-j` if CPU is available |
