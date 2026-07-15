# Bonus

## Bonus 1 — Result visualization

Use `optisans evaluate` to produce a fitness CSV, then generate plots with the scripts in `src/`:

```bash
python src/plot_fitness_evolution.py best_fitness_summary.csv --annotate
python src/d2o_vs_d.py gfp_clean_primus_out/
```

## Bonus 2 — Convergence study

Run the GA multiple times with different seeds:

```bash
for seed in 1 2 3 4 5; do
  optisans run gfp_clean.pdb -p 30 -e 5 -g 30 --seed $seed --d2o 0 --d2o 42 --d2o 100
done
```

Compare the convergence curves and the best genomes obtained.

## Bonus 3 — Full contrast scan (recycle)

Run a full scan from 0 to 100% D2O with a fixed pattern:

```bash
optisans recycle gfp_clean.pdb --d2o 42 --aa LEU,LYS,MET --step 1 --jobs 100
```

This generates 101 SANS curves and produces:
- `fitness_vs_d2o.png`: fitness vs D2O percentage
- `I0_vs_d2o_*.png`: intensity at q=0 vs D2O

## Bonus 4 — Search space restrictions

Limit the AAs the GA can modify via an INI file:

```ini
[RESTRICTIONS]
ALA = False
ARG = False
ASN = True
ASP = True
CYS = False
GLU = True
GLN = True
GLY = False
HIS = False
ILE = False
LEU = True
LYS = True
MET = True
PHE = False
PRO = False
SER = False
THR = False
TRP = False
TYR = False
VAL = False
```

Then run:

```bash
optisans run gfp_clean.pdb --config restrictions.ini -p 30 -e 5 -g 30
```

## Bonus 5 — Complete configuration file

All GA parameters can be stored in a `config.ini` file:

```ini
[POPULATION]
population_size = 60
elitism = 10
d2o_variation_rate = 10

[GENETIC]
mutation_rate = 0.2
crossover_rate = 0.6

[EXECUTION]
generations = 100
seed = 42
patience = 20

[RESTRICTIONS]
ALA = True
ARG = True
...

[FITNESS]
q_max = 0.3
ratio_threshold = 0.01

[D2O]
d2o = 0 42 100
```

## Bonus 6 — Performance and parallelism

- Increase `-j, --jobs` to speed up Pepsi-SANS if you have enough CPU cores.
- Use `--batch-script` to point to a customized version of `parallel_process_pdb.sh`.
- `--conc` lets you vary the concentration in mg/mL, which changes the incoherent scattering background.
