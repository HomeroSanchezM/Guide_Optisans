# Getting Started

## Key concepts

- Fitness: product of the areas between the simulated curve and each reference curve, multiplied by the signal-to-background ratio (Imax/background)^gamma.
- Default reference curves: protonated protein in H2O (D2O=0%) and protonated protein in D2O 100% (all labile H exchanged).

## Generated folder structure

After a genetic algorithm run, OptiSANS creates the following folders:

- `<pdb_stem>_deuterated_pdbs/` : generated deuterated PDB files
- `<pdb_stem>_primus_out/` : Pepsi-SANS results (.dat) and ref/ folder
- `<pdb_stem>_final_results/` : final GA result (best solution)
- `best_fitness_summary.csv` : best fitness per generation

PDB naming convention (required by `parallel_process_pdb.sh`):

- `_d2o42.pdb` => `--d2o 0.42`
- `_total_protonation.pdb` => `--d2o 0`
- `_total_deuteration.pdb` => `--d2o 1`

## Unified `optisans` CLI tour

All commands are run via `optisans` (or `pixi run optisans` outside a pixi shell).

```bash
optisans --help
```
And to have the option of a given subcommand.

```bash
optisans deuterate --help
```

### List amino acids

```bash
optisans aa
```

Displays the 20 standard amino acids with their 3-letter code, 1-letter code, and full name.

### Deuterate a PDB

```bash
optisans deuterate input.pdb -o output.pdb --d2o 50 --aa LEU,LYS,MET
```

Main options:
- `-o, --output`: output PDB path
- `--d2o`: D2O percentage for labile H (0-100)
- `--aa`: list of AAs to deuterate, separated by spaces or commas
- `--all`: deuterate all AAs
- `--config`: INI configuration file

### Run a SANS simulation (fixed contrast)

```bash
optisans simulate deuterated.pdb
```

By default, D2O is extracted from the filename using the `_d2oXX.pdb` convention. You can force a value:

```bash
optisans simulate deuterated.pdb --d2o 42
```

Useful options:
- `-j, --jobs`: number of parallel jobs (default 150)
- `--conc`: concentration in mg/mL (default 2.5)
- `--batch-script`: path to `parallel_process_pdb.sh`

### Contrast scan (recycle)

```bash
optisans recycle protein.pdb --d2o 42 --aa LEU,LYS
```

Generates all PDBs from D2O=0% to 100%, runs Pepsi-SANS, evaluates fitness, and produces fitness vs D2O and I(0) vs D2O plots.

Options:
- `--step`: D2O scan step (default 1 = every percentage point)
- `--output-dir`: custom output directory
- `--no-default-ref`: do not generate default references

### Evaluate existing results

```bash
optisans evaluate results_dir/
```

Re-evaluates fitness of `.dat` files in a directory without re-running Pepsi-SANS.

Options:
- `--q-max`: q truncation limit (default 0.3 A^-1)
- `--ratio-threshold`: minimum ratio (default 0.01)
- `--gamma`: ratio exponent (default 2)

### Run the full genetic algorithm

```bash
optisans run protein.pdb -p 30 -e 5 -g 50 --seed 42
```

Runs the full GA: population generation, PDB creation, SANS simulation, evaluation, and evolution over N generations.

Main options:
- `-p, --population-size`: population size (multiple of 3, default 120)
- `-e, --elitism`: number of preserved elites (default 10)
- `-g, --generations`: number of generations (default 500)
- `--d2o-var`: max D2O mutation amplitude per mutation (0-100, default 5)
- `--seed`: random seed for reproducibility
- `--d2o`: lock explored D2O values (e.g. `--d2o 0 --d2o 42 --d2o 100`)
- `--patience`: early stopping if no improvement for N generations (default 50, 0 = disabled)
- `--q-max`, `--ratio-threshold`, `--gamma`: fitness parameters
- `--ref`: additional PDB references
- `--no-default-ref`: do not create default references
- `--config`: INI configuration file


