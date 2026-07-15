# Installation

## Overview

OptiSANS depends on the following components:

- Pepsi-SANS 3.0 
- Python >= 3.11
- Python packages: numpy ; dataclasses ; biopython ; gemmi ; matplotlib
- GNU parallel 
- pixi (reproducible environment manager)

## Recommended installation with pixi

This project uses pixi to manage all dependencies in a reproducible environment.

Install pixi with

```bash
curl -fsSL https://pixi.sh/install.sh | sh
```

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

## Resources

- Pepsi-SANS documentation: https://www.ill.eu/sites/ccsl/ffax/pepsi-sans/
- pixi documentation: https://pixi.sh
- OptiSANS repository: https://github.com/HomeroSanchezM/OptiSANS
