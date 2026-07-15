# Practical Example : Deuteration and SANS Simulation

Objective: deuterate a GFP PDB according to given conditions, generate the corresponding SANS curves, and evaluate their fitness.

## Experimental conditions

The conditions are defined in [conditions.md](conditions.md).

## Procedure

### Step 1 : Deuterate the PDB

Generate a deuterated PDB with a fixed pattern:

```bash
optisans deuterate data/gfp.pdb -o results/gfp_deutered_d2o42.pdb --d2o 42 --aa LEU,LYS,MET
```

This creates `gfp_deutered_42.pdb` with:
- LEU, LYS and MET residues deuterated
- 42% of labile H exchanged to D (D2O = 42%)

Check logs in case of errors. The `--verbose` option shows detailed operation logs.

### Step 2 : Generate the reference PDB

Generate the D2O=0 and D2O=100 DPBs in a ref folder

```bash
optisans deuterate data/gfp.pdb -o results/ref/gfp_total_deuteration.pdb --d2o 100 
optisans deuterate data/gfp.pdb -o results/ref/gfp_total_protonation.pdb --d2o 0 
```

### Step 3 : Run the Pepsi-SANS simulation

Execute Pepsi-SANS on the deuterated PDB:

```bash
optisans simulate results/ 
```

This calls `parallel_process_pdb.sh`, which runs Pepsi-SANS in parallel and produces a `.dat` file in `<folder_stem>_primus_out/`.

If D2O is not specified via `--d2o`, it is extracted from the filename.

### Step 4 : Evaluate the fitness of the deuterated PDB

To evaluate an existing folder with ready `.dat` files:

```bash
optisans evaluate results_primus_out/ --q-max 0.3 --ratio-threshold 0.01 --gamma 2
```

This produces a summary of fitness scores.

### Interpretation

The `.dat` file produced by Pepsi-SANS contains the columns `q` and `I(q)` (scattered intensity). Fitness is computed as:

```
fitness = product(areas_i) * (Imax / background)^gamma
```

where `areas_i` is the area between the simulated curve and each reference curve, and `background = -0.0117 * d2o_percent + 1.25`.

A high fitness means the curve is far from both references: it therefore carries a lot of contrast information.

## Expected results

See [expected_results.md](expected_results.md).
