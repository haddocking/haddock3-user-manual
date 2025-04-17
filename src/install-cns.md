## Install CNS

HADDOCK is using [Crystallography & NMR System (CNS)](http://cns-online.org/v1.3/) as a core computing engine.
CNS is a FORTRAN66 code that must be compiled on your machine, for your own hardware.

### Pre-compiled binaries

To simplify the installation procedure of haddock3, we now provide pre-compiled CNS binaries, that are automatically installed when you run `pip install haddock3`.
Therefore there should be no need of compiling it yourself, which was one of the major issue related to the installation of HADDOCK.

**DISCLAMER**:
By running this command, you will download a compiled executable of CNS (Crystallographic and NMR System) which is free of use for non-profit applications.
For commercial use, it is your own responsibility to have a proper license.
For details refer to [the DISCLAIMER file](https://github.com/haddocking/haddock3/blob/main/DISCLAIMER.md) in the HADDOCK3 repository.

### Compiling CNS on your own

Please see the [up-to-date installation procedure of CNS here](https://github.com/haddocking/haddock3/blob/main/docs/CNS.md), where you will find specific guides and troubleshooting sections.

Once compiled, you need to replace the executable located in the haddock package in virtual environnement (`haddock/bin/`).

Here is an example:
```bash
# Given you have created a virtual env named .haddock3-env
python3.12 -m venv .haddock3-env
# and already installed haddock3
pip install .
# You can replace the cns executable by your own newly generated executable in the site-packages/haddock/bin/ directory
cp my-own-cns-executable .haddock3-env/lib/python3.12/site-packages/haddock/bin/cns
```