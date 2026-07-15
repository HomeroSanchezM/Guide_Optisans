# Practical Example — Deuteration and SANS Simulation

Objective: deuterate a GFP PDB according to given conditions, generate the corresponding SANS curves, and evaluate their fitness.

## Experimental conditions

The conditions are defined in [conditions.md](conditions.md).

## Procedure

### Step 1 — Deuterate the PDB

Generate a deuterated PDB with a fixed pattern:

```bash
optisans deuterate gfp_clean.pdb -o gfp_deutered_42.pdb --d2o 42 --aa LEU,LYS,MET
```

This creates `gfp_deutered_42.pdb` with:
- LEU, LYS and MET residues deuterated
- 42% of labile H exchanged to D (D2O = 42%)

Check logs in case of errors. The `--verbose` option shows detailed operation logs.

### Step 2 — Run the Pepsi-SANS simulation

Execute Pepsi-SANS on the deuterated PDB:

```bash
optisans simulate gfp_deutered_42.pdb --d2o 42 -j 50
```

This calls `parallel_process_pdb.sh`, which runs Pepsi-SANS in parallel and produces a `.dat` file in `<pdb_stem>_primus_out/`.

If D2O is not specified via `--d2o`, it is extracted from the filename. If the file does not contain a `_d2oXX` pattern, the simulation runs without the `--d2o` flag and Pepsi-SANS uses its default value.

### Step 3 — Generate references and evaluate

To evaluate the curve, you need references in `ref/`. Two options:

Option A — use `recycle` which automatically generates references:

```bash
optisans recycle gfp_clean.pdb --d2o 42 --aa LEU,LYS,MET --step 100
```

With `--step 100`, only D2O=0 and D2O=100 are generated, which is enough to obtain the two default references. The full scan is not run.

Option B — evaluate an existing folder with ready `.dat` files:

```bash
optisans evaluate gfp_deutered_42_primus_out/ --q-max 0.3 --ratio-threshold 0.01 --gamma 2
```

This produces a summary of fitness scores.

### Step 4 — Interpretation

The `.dat` file produced by Pepsi-SANS contains the columns `q` and `I(q)` (scattered intensity). Fitness is computed as:

```
fitness = product(areas_i) * (Imax / background)^gamma
```

where `areas_i` is the area between the simulated curve and each reference curve, and `background = -0.0117 * d2o_percent + 1.25`.

A high fitness means the curve is far from both references: it therefore carries a lot of contrast information.

## Expected results

See [expected_results.md](expected_results.md).
