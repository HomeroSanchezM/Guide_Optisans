# Expected Results : Deuteration and SANS Simulation

## Generated files

After execution, you should obtain:

- `results/gfp_deutered_d2o42.pdb` : deuterated PDB with LEU, LYS, MET deuterated and 42% labile exchange.
- `results_primus_out/gfp_deutered_d2o42.dat` : simulated SANS curve.
- `results/ref/` : reference PDB files 
- `results_primus_out/ref/` : reference SANS curves.

## Quick troubleshooting

| Problem | Likely cause | Solution |
|----------|---------------|----------|
| No `.dat` in `_primus_out/` | Pepsi-SANS failed | Run with `--verbose`, check that `Pepsi-SANS-Linux/Pepsi-SANS` is executable |
| Ratio is 0 | `Imax < ratio_threshold` or D2O not detected | Check the PDB filename and `--d2o` flag |
| Null fitness | missing references in `ref/` | Generate references with `recycle` or place `.dat` files manually |
| `File not found` error | incorrect path | Use an absolute path or run from the correct directory |
