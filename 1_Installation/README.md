# Installation

## Overview

OptiSANS depends on the following components:

- Pepsi-SANS 3.0 (Linux executable in `./Pepsi-SANS-Linux/Pepsi-SANS`)
- Python >= 3.11
- Python packages: numpy >= 2.4.1, < 3 ; dataclasses >= 0.8, < 0.9 ; biopython >= 1.86, < 2 ; gemmi >= 0.7.4, < 0.8 ; matplotlib >= 3.10.8, < 4
- GNU parallel (for `parallel_process_pdb.sh`)
- pixi (reproducible environment manager)

## Option 1: Recommended installation with pixi

Clone the repository and enter the pixi environment:

```bash
git clone https://github.com/HomeroSanchezM/OptiSANS.git
cd OptiSANS
pixi shell
```

`pixi shell` automatically installs all dependencies in an isolated environment. The `optisans` CLI is then available directly. You can also use it without entering the shell via `pixi run optisans`.

Verification:

```bash
optisans --help
optisans aa
```

## Option 2: Manual installation

If pixi is not available on your machine, you can install the dependencies manually:

```bash
git clone https://github.com/HomeroSanchezM/OptiSANS.git
cd OptiSANS
python3 -m venv .venv
source .venv/bin/activate
pip install numpy>=2.4.1,<3 dataclasses>=0.8,<0.9 biopython>=1.86,<2 gemmi>=0.7.4,<0.8 matplotlib>=3.10.8,<4 typer
```

Install GNU parallel:

```bash
sudo apt install parallel   # Debian/Ubuntu
```

Verify that the Pepsi-SANS executable is present:

```bash
ls ./Pepsi-SANS-Linux/Pepsi-SANS
chmod +x ./Pepsi-SANS-Linux/Pepsi-SANS
```

## Resources

- Pepsi-SANS documentation: https://www.ill.eu/sites/ccsl/ffax/pepsi-sans/
- pixi documentation: https://pixi.sh
- OptiSANS repository: https://github.com/HomeroSanchezM/OptiSANS
