## Protein-protein shape-restrained docking


This example illustrate the use of a shape to guide the docking of two proteins. 
For this ambiguous restraints are defined from every second CA atom of the proteins to any shape beads with an upper limit of 5Å.

The ambiguous restraint file (`data/ambig-shape.tbl`) looks as follows:

```
assign (segid A and resi 1461 and (name CA or name BB)) (segid S) 5.000 5.000 0.000
assign (segid A and resi 1463 and (name CA or name BB)) (segid S) 5.000 5.000 0.000
assign (segid A and resi 1465 and (name CA or name BB)) (segid S) 5.000 5.000 0.000
assign (segid A and resi 1467 and (name CA or name BB)) (segid S) 5.000 5.000 0.000
assign (segid A and resi 1469 and (name CA or name BB)) (segid S) 5.000 5.000 0.000
assign (segid A and resi 1471 and (name CA or name BB)) (segid S) 5.000 5.000 0.000
assign (segid A and resi 1473 and (name CA or name BB)) (segid S) 5.000 5.000 0.000
...
```

These restraints will guide the proteins to overlap with the shape. This approach could be used for example to dock proteins into the SAXS-derived shape.

In this particular example the shape was derived directly from the atomic coordinates of the reference complex using K-mean clustering and asking for a number of clusters that matches the number of residues in the complex. The PDB file containing the shape looks like:

```REMARK   FILE 2R15_REFERENCE.PDB
REMARK   NUM PTS 415
ATOM      1 SHA  SHA S   1       8.886  13.558 100.283  0.00  0.00      S
ATOM      2 SHA  SHA S   2       8.592  48.158  32.789  0.00  0.00      S
ATOM      3 SHA  SHA S   3      13.869  78.257   7.957  0.00  0.00      S
ATOM      4 SHA  SHA S   4       8.031  43.329  69.966  0.00  0.00      S
ATOM      5 SHA  SHA S   5      26.948  33.204  42.831  0.00  0.00      S
ATOM      6 SHA  SHA S   6      16.100  49.149  54.708  0.00  0.00      S
ATOM      7 SHA  SHA S   7      13.475  -3.673  93.942  0.00  0.00      S
ATOM      8 SHA  SHA S   8      12.878  86.954  18.744  0.00  0.00      S
...
```

Each shape atom is a separte residue, with chainID/segidID `S`.


The data for the run can be found in the `data` directory, consisting of:

* `2r15_A.pdb`: The first protein, chain A from the reference complex.
* `2r15_B.pdb`: The second protein, chain B from the reference complex.
* `2r15_reference.pdb`: The reference complex.
* `shape.pdb`: The shape representation.
* `shape-ambig.tbl`: The ambiguous distance restraints between the proteins CA atoms and the shape atoms.


Four HADDOCK3 workflow examples are provided, for both all-atoms and coarse-grained docking scenarios, and a full run or test run with limited sampling:

* [docking-protein-protein-shape-CG-full.cfg](https://github.com/haddocking/haddock3/blob/main/examples/docking-protein-protein-shape/docking-protein-protein-shape-CG-full.cfg): Coarse-grained shaped-restrained docking - full run
* [docking-protein-protein-shape-CG-test.cfg](https://github.com/haddocking/haddock3/blob/main/examples/docking-protein-protein-shape/docking-protein-protein-shape-CG-test.cfg): Coarse-grained shaped-restrained docking - test run
* [docking-protein-protein-shape-full.cfg](https://github.com/haddocking/haddock3/blob/main/examples/docking-protein-protein-shape/docking-protein-protein-shape-full.cfg): All atoms shaped-restrained docking - full run
* [docking-protein-protein-shape-test.cfg](https://github.com/haddocking/haddock3/blob/main/examples/docking-protein-protein-shape/docking-protein-protein-shape-test.cfg): All atoms shaped-restrained docking - test run


The docking workflows are standard HADDOCK runs with rigidbody docking, flexible refinement, energy minimisation and clustering.
For the coarse-grained versions two additional modules are added: `topocg` to generate the coarse-grained representation and `cgtoaa` 
to convert back the CG models into their all atoms represenations.

The full, all atoms worklfow looks like:

```toml
# ====================================================================
# Protein-protein docking example with shape restraints

# directory in which the scoring will be done
run_dir = "run1-full"

# execution mode
mode = "local"
ncores = 50

# molecules to be docked
molecules =  [
    "data/2r15_A.pdb",
    "data/2r15_B.pdb",
    "data/shape.pdb",
    ]

# ====================================================================
# Parameters for each stage are defined below, prefer full paths
# ====================================================================
[topoaa]

[rigidbody]
tolerance = 20
ambig_fname = "data/ambig-shape.tbl"
mol_shape_3 = true
mol_fix_origin_3 = true

[caprieval]
reference_fname = "data/2r15_reference.pdb"

[seletop]

[flexref]
tolerance = 20
ambig_fname = "data/ambig-shape.tbl"
mol_shape_3 = true
mol_fix_origin_3 = true

[caprieval]
reference_fname = "data/2r15_reference.pdb"

[emref]
tolerance = 20
ambig_fname = "data/ambig-shape.tbl"
mol_shape_3 = true
mol_fix_origin_3 = true

[caprieval]
reference_fname = "data/2r15_reference.pdb"

[clustfcc]
min_population = 4

[seletopclusts]
top_models = 4

[caprieval]
reference_fname = "data/2r15_reference.pdb"

# ====================================================================

```