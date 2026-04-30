## Polycomb repressive complex 1 (PRC1) Ubiquitilation module bound to the Nucleosome

This example illustrates the use of HADDOCK3 in coarse-grained mode to model the interaction 
between the Polycomb repressive complex 1 (PRC1) ubiquitination module and a nucleosome particle.


The coarse-graining implementation in HADDOCK3, illustrated among others with this particular complex is described in the following article:

* R. Versini, V.G.P. Reys, A. Kravchenko, R.V. Honorato and **A.M.J.J. Bonvin**. [Integrating the MARTINI2 Coarse-Grained Force Field into HADDOCK3 for Faster Modelling of Large Biomolecular Complexes](https://doi.org/10.64898/2026.04.25.720800). _BioRxiv_ 10.64898/2026.04.25.720800 (2026)



The modelling starts from the unbound crystal structure of the enzymatic complex PRC1 (PDB code: 3RPG) and the nucleosome particle (PDB cod
e: 3LZ0) with default HADDOCK3 sampling.

The docking is guided by interaction restraints reported in the literature available at the time of CAPRI Round 31. These include a single
 distance restraint (with an upper limit of 2Å) between the SG atom of the catalytic cysteine (Cys85) of PRC1 and the NZ atom of Lys118 or
Lys119 on chain C of histone H2A, the ubiquitination site. In addition, mutagenesis data identifying key PRC1 residues (K62 of chain B, R64
 of chain B, K97 of chain C, and R98 of chain C) as essential for nucleosome binding were incorporated. AIRs were defined by assigning thes
e PRC1 residues as active and all solvent-accessible histone residues on the side of the nucleosome where the catalytic cysteine is found a
s passive, filtered for solvent accessibility.

The example directory contains the input data, HADDOCK3 workflow and the results of the CG docking run for this system:


The HADDOCK-ready input PDB files (those were processed to have a single chain with non-overlapping residue numbers per molecule to be dock
ed) can be found in the `data` directory:

* `3lz0_merged_A.pdb`: Nucleosome particle containing the histones and the DNA
* `3rpg_clean.pdb`: The unbound crystal structure of the enzymatic complex PRC1
* `4r8p_reference.pdb`: The reference crystal structure of the complex (used for CAPRI quality metrics evaluation)

The restraints files in the `data` directory are:

* `ambig.tbl`: The ambiguous interaction restraints (AIR) file used to drive the docking created based on the mutagenesis data and the solv
ent accessible residues of the histones)
* `lys-cys-linkage.tbl`: The restraint defining the linkage between the catalytic cystein of PRC1 and the NZ atom of Lysines 602 or 603 (HA
DDOCK-ready residue numbering).
* `unambig.tbl`: Restraints defined to keep bodies connected during the flexible refinement (generated using the `haddock3 restrain_bodies`
 command). This file also contains the Cys-Lys restraint.


Three workflow examples are provided:

* `docking-nucleosome-PRC1-AA-full.cfg`: Full docking workflow in all atom mode
* `docking-nucleosome-PRC1-CG-full.cfg`: Full docking workflow using the coarse-grained mode
* `docking-nucleosome-PRC1-CG-test.cfg`: Test (short) docking workflow using the coarse-grained mode


The full CG workflow looks like:

```toml
# ====================================================================
# Protein-protein docking example with NMR-derived ambiguous interaction restraints
# ====================================================================

# directory in which the scoring will be done
run_dir = "run1-full-CG"

# execution mode
mode = "batch"   #using slurm - change to local for local execution and adapt the number of cores
queue = "short"
queue_limit = 400
ncores = 10

# molecules to be docked
molecules =  [
    "data/3lz0_merged_A.pdb",
    "data/3rpg_clean.pdb"
    ]

postprocess = True 
debug = True

# ====================================================================
# Parameters for each stage are defined below, prefer full paths
# ====================================================================

[topoaa]

[topocg]
cgffversion = "martini2"

[rigidbody]
ambig_fname = "data/ambig.tbl"
unambig_fname = "data/unambig.tbl"
randremoval = False
tolerance = 10
sampling = 1000

[caprieval]
reference_fname = "data/4r8p_reference.pdb"
fnat_cutoff = 7.0 # cutoff for coarse-grained structures

[seletop]
select = 200

[caprieval]
reference_fname = "data/4r8p_reference.pdb"
fnat_cutoff = 7.0 # cutoff for coarse-grained structures

[flexref]
ambig_fname = "data/ambig.tbl"
unambig_fname = "data/unambig.tbl"
randremoval = False
tolerance = 5

[caprieval]
reference_fname = "data/4r8p_reference.pdb"
fnat_cutoff = 7.0 # cutoff for coarse-grained structures

[cgtoaa]

[emref]
ambig_fname = "data/ambig.tbl"
unambig_fname = "data/unambig.tbl"
randremoval = False
tolerance = 5

[caprieval]
reference_fname = "data/4r8p_reference.pdb"

[clustfcc]

[seletopclusts]

[caprieval]
reference_fname = "data/4r8p_reference.pdb"

# ====================================================================

```


__Note__ the increase cutoff used in `caprieval` to identigy contacts when in coarse-grained mode.

Compared to the all atoms workflow, the CG workflow has two additional modules: 

* `topocg` following the `topoaa` module and required to generate the CG representations and the associated restraints needed to convert back the models to all-atoms representations
* `cgtoaa` following the `flexref` stage to convert back the models to all-atoms representations.


