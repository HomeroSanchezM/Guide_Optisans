# Expected Results — Deuteration and SANS Simulation

## Generated files

After execution, you should obtain:

- `gfp_deutered_42.pdb` — deuterated PDB with LEU, LYS, MET deuterated and 42% labile exchange.
- `gfp_deutered_42_primus_out/gfp_deutered_42.dat` — simulated SANS curve.
- `gfp_deutered_42_primus_out/ref/` — reference curves.

## Fitness score interpretation

The score displayed by `optisans evaluate` follows the formula:

```
fitness = product(areas_i) * ratio^gamma
```

- `areas_i`: area between the simulated curve and each reference, computed by Simpson integration.
- `ratio`: `Imax / background`, with `background = -0.0117 * d2o_percent + 1.25`.
- `gamma`: exponent (2 by default, quadratic).

For the practical example:

- The ratio must be greater than `ratio_threshold = 0.01` for the curve to be accepted.
- Fitness increases when the curve moves away from the references: the more informative the deuteration, the higher the fitness.
- If fitness is low, check that the references are present in `ref/` and that the `.dat` file contains two columns `q` and `I`.

## Quick troubleshooting

| Problem | Likely cause | Solution |
|----------|---------------|----------|
| No `.dat` in `_primus_out/` | Pepsi-SANS failed | Run with `--verbose`, check that `Pepsi-SANS-Linux/Pepsi-SANS` is executable |
| Ratio is 0 | `Imax < ratio_threshold` or D2O not detected | Check the PDB filename and `--d2o` flag |
| Null fitness | missing references in `ref/` | Generate references with `recycle` or place `.dat` files manually |
| `File not found` error | incorrect path | Use an absolute path or run from the correct directory |
