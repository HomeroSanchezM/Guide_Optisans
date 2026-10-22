# Expected Results : Genetic Algorithm

## Generated files

After GA execution, the following folders are created:

- `gfp_deuterated_pdbs/` : one PDB per chromosome and per generation
- `gfp_primus_out/` : simulated SANS curves (.dat) and ref/ folder
- `gfp_final_results/` : best solution and fitness history
  - `pdb/` : best deuterated PDB (`gfp_best_fitness.pdb`) and reference PDBs in `pdb/ref/`
  - `sans_simulation/` : best simulated SANS curve (`gfp_best_fitness.dat`) and reference curves in `sans_simulation/ref/`
  - `gfp_best_fitness.csv`
  - `config.ini`

## Reading the best genome

The file `gfp_best_fitness.csv` contains:

- `generation`, `index`: generation and number of the best chromosome
- `fitness`: final fitness score
- `d2o_percent`: optimal D2O percentage
- `n_deuterated_aa`: number of deuterated AAs (genes set to True)
- `ratio`: Imax/background ratio
- `%D`, `%Non_labile_D%` : Percentage of total deuterium and non labile deuterium
- `deuterated_aa_list` : List of the deuterated AA


## Quick troubleshooting

| Problem | Likely cause | Solution |
|----------|---------------|----------|
| Null fitness after 30 generations | population too small or missing references | increase `-p`, check `ref/` |
| No convergence | `--d2o` too restrictive or `--d2o-var` too small | broaden D2O values or increase `--d2o-var` |
| `multiple of 3` error | `population_size` is not a multiple of 3 | use 30, 60, 90, etc. |
| GA is slow | too many parallel jobs or generations too long | reduce `-g` or increase `-j` if CPU is available |
