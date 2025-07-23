# Obtaining a set of interacting resiudes in HADDOCK

Obtaining a list of interacting residues to be used as input to HADDOCK may require some prior investigations.

Those restraints can come from various sources, including knowledge about the target, information from experimental data (i.e.: NMR chemical shift, cross-link experiments, FRET, etc..), or bioinformatic predictions.

In the following sections, we will be describing how to obtain lists of residues potentially involved in the interaction.

## Mining interacting residues from experimental data

Sometimes, you may have access to relevent information coming directly from the experiment, that you did youself or published by someone else.

Here are some tutorials (HADDOCK2.X series), explaining how such kind of data can be used to derive ambiguous interaction restraints:

- [From NMR chemical shifts perturbation](https://www.bonvinlab.org/education/NMRMolmod/)
- [From cross-linking experiments](https://www.bonvinlab.org/education/HADDOCK24/HADDOCK24-Xlinks/)


## Use of bioinformatic interface predictions

In the absence of any experimental information on the interaction surfaces, you might want to try to predict them based on sequence conservation and other properties.
We have developed for this purpose interface mining [ARCTIC-3D](https://wenmr.science.uu.nl/arctic3d/) and prediction software called [WHISCY](https://wenmr.science.uu.nl/whiscy) and [CPORT](https://alcazar.science.uu.nl/services/CPORT).
They have been designed to provide an easy interface to HADDOCK and will output, among others, lists of active and passive residues for HADDOCK.
CPORT is a meta-predictor that integrates results from various other servers.

For more information refer to:

- [ARCTIC-3D (online webserver)](https://wenmr.science.uu.nl/arctic3d/): M. Giulini, R.V. Honorato, J.L. Rivera and A.M.J.J Bonvin. [ARCTIC-3D: automatic retrieval and clustering of interfaces in complexes from 3D structural information.](https://doi.org/doi:10.1038/s42003-023-05718-w) _Commun Biol_ 7, 49 (2024).

- S.J. de Vries, A.D.J. van Dijk and A.M.J.J. Bonvin. [WHISCY: WHat Information does Surface Conservation Yield? Application to data-driven docking.](https://doi.org/doi:10.1002/prot.20842), _Proteins: Struc. Funct. & Bioinformatics_, **63**, 479-489 (2006).

- S.J. de Vries and A.M.J.J. Bonvin. [CPORT: a Consensus Interface Predictor and its Performance in Prediction-driven Docking with HADDOCK.](https://doi.org/doi:10.1371/journal.pone.0017695), _PlosONE_, **6** e17695 (2011).

Other predictors do exist, such as:

- [PeSTo (online webserver)](https://pesto.epfl.ch/): Lucien F. Krapp, _et al,_. [PeSTo: parameter-free geometric deep learning for accurate prediction of protein binding interfaces](https://www.nature.com/articles/s41467-023-37701-8), _Nature Communications_, 14, 2175 (2023).


