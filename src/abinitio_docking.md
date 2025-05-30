# Ab-initio / naive docking protocols

While HADDOCK is meant to use information from experiments, literature, or bioinformatic predictions to guide the sampling during the docking, sometimes such data is not available.
For these reasons, dedicated parameters can be turned **on** to perform _ab-initio_ docking.

Three different ways of doing _ab-initio_ docking in haddock3 are discussed below.

## Prior considerations

- Ab-initio docking typically involves limited, if any, prior information on how the various chains involved should interact. As a result, producing good solutions relies heavily on a trial-and-error approach. Thus, to enhance the likelihood of generating good models, we strongly advise increasing the sampling at the `[rigidbody]` docking stage (by tuning the `sampling` parameter).
- The next three _ab-initio_ docking solutions described below are incompatible with each other, and you should not turn **on** multiple of them at the same time.

## Center of mass restraints

Turning **on** the center of mass restraints parameter (`cmrest = true`), will automatically generate restraints between the centers of masses of the different chains present in the system, and use these restraints during the docking.

This parameter goes together with the `cmtight` parameter, which controls how the upper limit distance is defined for the center of mass restraints.
To calculate the upper distance limit for the restraints, the height, width, and depth of each molecule are first determined. Technically, each molecule is aligned along its principal (i.e. longest) components, and the x, y, and z dimensions are measured. Next:

- If `cmtight=true`: The 'molecule distance' for each molecule is calculated as the average of the two smallest dimensions, each divided by 2. For example:

![equ](https://latex.codecogs.com/gif.latex?Molecule\;Distance = \frac{1}{2} \left( \frac{\text{width}}{2} + \frac{\text{depth}}{2} \right))

- If `cmtight=false`: The 'molecule distance' is the average of all three half-dimensions:

![equ](https://latex.codecogs.com/gif.latex?Molecule\;Distance = \frac{1}{3} \left( \frac{\text{height}}{2} + \frac{\text{width}}{2} + \frac{\text{depth}}{2} \right))


- For DNA, RNA, small ligands, or glycans: The 'molecule distance' is set to 0.  
  The effective upper distance limit for the center of mass distance restraint is defined as the sum of the molecule distances of all molecules involved.

Lastly, the strength of the center of mass restraints can be controlled via the force constant (`kcm`)

`cmrest`, `cmtight` and `kcm` parameters are accessible in `[rigidbody]`, `[flexref]` and `[mdref]` modules.

Please note that setting `cmrest = true` is suitable for globular structures, but may deform other types of molecules, e.g. fibrous proteins, long bDNA etc., as restraint will be defined to the center of the molecule.

## Random Ambiguous Restraints

Another solution for ab-initio docking is to generate random ambiguous restraints (AIRs).
This is performed by turning **on** the `ranair` parameter (`ranair = true`) in the `[rigidbody]` module.
When ranair is turned on:

- During the rigid-body sampling, residues on the surface of each chain are randomly selected, along with surrounding ones, to define a patch.
- Ambiguous restraints are then generated between these patches, and rigid-body minimization is performed.

`ranair` parameter is limited to the docking of two chains only, and no other type of restraints will be considered, even if specified in the configuration file.

**_Note_** that during the later stages of the docking workflow (e.g., [flexref], [emref], [mdref]), it is advisable to enable the `contactairs = true` parameter to ensure the molecules remain held together at the interface. This setting defines restraints between thwe residues within a 5Å distance between molecules. However, be aware this may generate a large number of restraints, potentially slowing down computations.

## Surface restraints

An alternative solution for ab-initio docking is to turn **on** the `surfrest` parameter (`surfrest = true`).
By doing so, surface residues are identified, and contact restraints between these residues across docking partners are generated on the fly.
These restraints are defined as ambiguous distance restraints between all backbone atoms (CA, BB, or N1) of the two molecules. For small ligands, all atoms are considered.
If fewer than 3 CA and P atoms are found, all atoms are selected instead.
The upper distance limit is set to 7Å for standard molecules and 4.5Å for small ligands.

Such restraints can be particularly useful in multi-body (N>2) docking to ensure that all molecules are in contact and thus promote compactness of the docking solutions.
Similarly to the [random AIRs](#random-ambiguous-restraints), surface contact restraints can be used in _ab-initio_ docking. In such a case it is important to have sufficient sampling of the random starting orientations, which significantly increases the number of structures generated by the rigid-body docking.

**_Note_** that this option is computationally more expensive than [center of mass restraints](#center-of-mass-restraints) and [random AIRs](#random-ambiguous-restraints), as the number of restraints grows exponentially with the number of residues in the system.
Also, because of the high number of restraints, the physico-chemical components of the scoring function can be masked by the noise of the AIRs component.
Therefore setting the weight of the AIR component to 0 (`w_air = 0`) could help the scoring function to better decipher between model conformations.

This parameter goes along with its force constant `ksurf`, which can be tuned to control the strength of the surface restraints.
