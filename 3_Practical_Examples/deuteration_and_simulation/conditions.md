# Conditions — Deuteration and SANS Simulation

## Protein

- Input file: `gfp_clean.pdb`
- All hydrogens must be explicit and protonated in the PDB.

## Deuteration pattern

| Parameter | Value |
|-----------|--------|
| D2O (%) | `42` |
| Deuterated amino acids | `LEU`, `LYS`, `MET` |
| Concentration (mg/mL) | `2.5` |
| q_max (A^-1) | `0.3` |
| ratio_threshold | `0.01` |
| gamma | `2` |
| Parallel jobs | `50` |

## Commands

```bash
# 1. Deuterate
optisans deuterate gfp_clean.pdb -o gfp_deutered_42.pdb --d2o 42 --aa LEU,LYS,MET --verbose

# 2. Simulate
optisans simulate gfp_deutered_42.pdb --d2o 42 -j 50

# 3. Generate references (optional)
optisans recycle gfp_clean.pdb --d2o 42 --aa LEU,LYS,MET --step 100

# 4. Evaluate
optisans evaluate gfp_deutered_42_primus_out/ --q-max 0.3 --ratio-threshold 0.01 --gamma 2
```

## Success criteria

- `gfp_deutered_42.pdb` is created without errors.
- The `.dat` file is present in `gfp_deutered_42_primus_out/`.
- The `ref/` folder contains at least one reference curve.
- The `evaluate` command outputs a non-zero fitness score.
