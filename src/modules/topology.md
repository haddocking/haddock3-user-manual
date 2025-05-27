# Topology modules

- [`[topoaa]` module](#topoaa-module)

## `[topoaa]` module

The `[topoaa]` module is dedicated to the generation of CNS compatible parameters (.param) and topologies (.psf) for each of the input structures.

It will:
- Detect missing atoms, including hydrogens
- Re-build them when missing
- Build and write out topologies (`.psf`) and coordinates (`.pdb`) files

This module is a prerequisite to run any downstream modules using CNS.
Having access to parameters and topology is mandatory for any kind of EM/MD related tasks.
Therefore this is the reason why the module `[topoaa]` is often used as first module in a workflow.

Note that for non-standard bio-molecules (apart from standard amino-acids, some modified ones, DNA, RNA, ions and carbohydrates ... see [detailed list of supported molecules](https://wenmr.science.uu.nl/haddock2.4/library)), such as small-molecules, parameters and topology must be obtained and provided by the user, as there is currently no built-in solution to generate them on the fly.

More information about `[topoaa]` parameters can be accessed [here](https://bonvinlab.org/haddock3/modules/topology/haddock.modules.topology.topoaa.html#default-parameters) or retrieved by running:

```bash
haddock3-cfg -m topoaa
```

Here an example configuration file snapshot of a typical execution of the
`[topoaa]` module in which a user specifies the protonation state of the histidine
residues:

```toml
# ...
molecules = [
 "1abc.pdb",
 "2xyz.pdb"
]

[topoaa]
autohis = false
[topoaa.mol1]
nhisd = 0
nhise = 1
hise_1 = 75
[topoaa.mol2]
nhisd = 1
hisd_1 = 76
nhise = 1
hise_1 = 15

# Workflow continues
# ...
```

The `[topoaa.mol1]` in square brackets is **not** as module, but allows to specify `topoaa` parameters for a given molecule.
In this case (`mol1`), the parameters will be applied to the first molecule in the list of input molecules (`"1abc.pdb"`).

### Notable parameters

- `autohis`: If set to `false`, you will need to specify the protonation states of histidines manually. 

#### Parameters specific to each molecule

- `[topoaa.molX]`: Allows the definition of specific `topoaa` parameters for molecule X.
  - `nhisd`, `nhise` allow to define the number of `HISD`, `HISE` in the molecule.
  - `hisd_Y`, `hise_Y`, allow to define which residue needs to be modified (e.g: `hisd_1 = 3` means that we are defining the first HISD residue, and this residue has residue index of 3 in the file).
  - `charged_nter`, `charged_cter` allow to define the state of Nter and Cter residues.
  - `5_phosphate`: Allows to define the state of the 5' end of nucleic acids sequences. If set to `true`, 5' end will be a phosphate group. Otherwise it will be an OH. (default false).

<hr>
