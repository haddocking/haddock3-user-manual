# Scoring modules

- [`[emscoring]` module](#emscoring-module)
- [`[mdscoring]` module](#mdscoring-module)
- [prodigy modules](#prodigy-scoring-modules)
  - [`[prodigyprotein]`](#prodigyprotein-module)
  - [`[prodigyligand]`](#prodigyligand-module)
- [`[deeprank]` module](#deeprank-module)

## `[emscoring]` module

EM scoring module.

This module performs energy minimization and scoring of the models generated in
the previous step of the workflow. No restraints are applied during this step.

The default HADDOCK scoring function in the `[emscoring]` module is therefore the following:

![equ](https://latex.codecogs.com/gif.latex?HS=1.0E_{vdw}&plus;0.2E_{elec}&plus;0.0E_{air}&plus;1.0E_{desolv})

For a detailed explanation of the components of the scoring function, please have a look [here](../haddocking.md#haddock-scoring-function).

#### Notable parameters

The most important parameters for the `[emscoring]` module are:

- `nemsteps`: number of energy minimization steps
- `per_interface_scoring`: output per interface scores in the PDB header (default: False)


More information about `[emscoring]` parameters can be accessed [here](https://bonvinlab.org/haddock3/src/modules/scoring/haddock.modules.scoring.emscoring.html#default-parameters) or retrieved by running:

```bash
haddock3-cfg -m emscoring
```

<hr>

## `[mdscoring]` module

MD scoring module.

This module will perform a short MD simulation on the input models and
score them. No restraints are applied during this step.

The same scoring function as in the `[emscoring]` module is used:

![equ](https://latex.codecogs.com/gif.latex?HS=1.0E_{vdw}&plus;0.2E_{elec}&plus;0.0E_{air}&plus;1.0E_{desolv})

#### Notable parameters

The most important parameters for the `[mdscoring]` module are:

- `nemsteps`: number of energy minimization steps
- `per_interface_scoring` : output per interface scores in the PDB header (default: False)
- `waterheatsteps`: number of MD steps for heating up the system
- `watersteps`: number of MD steps at 300K
- `watercoolsteps` : number of MD steps for cooling down the system

More information about `[mdscoring]` parameters can be accessed [here](https://bonvinlab.org/haddock3/src/modules/scoring/haddock.modules.scoring.mdscoring.html#default-parameters) or retrieved by running:

```bash
haddock3-cfg -m mdscoring
```

<hr>

## PRODIGY scoring modules

Two modules are using the PRODIGY methods for the evaluation of binding affinity.
As this scoring is specific to either proteins or ligands, two modules are available, and should be used depending on which system you are working on:
- [`[prodigyprotein]`](#prodigyprotein-module): for the prediction of protein-protein binding affinities using PRODIGY
- [`[prodigyligand]`](#prodigyligand-module): for the prediction of protein-ligand binding affinities using PRODIGY-lig


### `[prodigyprotein]` module

Protein-protein binding affinity prediction using PRODIGY.

This module performs scoring of protein-protein complexes using PRODIGY ([GitHub](https://github.com/haddocking/prodigy), [PyPI package](https://pypi.org/project/prodigy-prot/)).

Note that this approach is limited to protein-protein interactions containing standard amino-acids.

A detailed explanation about PRODIGY can be found in published research articles:

- Xue L, Rodrigues J, Kastritis P, Bonvin A.M.J.J, Vangone A.: PRODIGY: a web server for predicting the binding affinity of protein-protein complexes. Bioinformatics (2016) ([10.1093/bioinformatics/btw514](https://doi.org/10.1093/bioinformatics/btw514))
- Anna Vangone and Alexandre M.J.J. Bonvin: Contacts-based prediction of binding affinity in protein-protein complexes. eLife, e07454 (2015) ([10.7554/eLife.07454](https://doi.org/10.7554/elife.07454))
- Panagiotis L. Kastritis , João P.G.L.M. Rodrigues, Gert E. Folkers, Rolf Boelens, Alexandre M.J.J. Bonvin: Proteins Feel More Than They See: Fine-Tuning of Binding Affinity by Properties of the Non-Interacting Surface. Journal of Molecular Biology, 14, 2632–2652 (2014). ([10.1016/j.jmb.2014.04.017](https://doi.org/10.1016/j.jmb.2014.04.017))


#### Notable parameters

The most important parameters for the `[prodigyprotein]` module are:

- `chains`: List of chains to be scored. If left empty, all inter-chains contacts will be considered for the final prediction. In specific cases, for example antibody-antigen complexes, some chains should be considered as a single molecule. Use the chains parameter to provide a list of chains that should be considered for the calculation. Use commas to include multiple chains as part of a single group.
  - ["A", "B"]      => Contacts calculated (only) between chains A and B.
  - ["A,B", "C"]    => Contacts calculated (only) between chains A and C; and B and C.
  - ["A", "B", "C"] => Contacts calculated (only) between chains A and B; B and C; and A and C.
- `to_pkd`: Converts predicted binding affinity values to pKd values.


More information about `[prodigyprotein]` parameters can be accessed [here](https://bonvinlab.org/haddock3/src/modules/scoring/haddock.modules.scoring.prodigyprotein.html#default-parameters) or retrieved by running:

```bash
haddock3-cfg -m prodigyprotein
```


### `[prodigyligand]` module

This module performs binding affinity prediction of protein-ligand complexes using PRODIGY-lig ([GitHub](https://github.com/haddocking/prodigy-lig), [PyPI package](https://pypi.org/project/prodigy-lig/)).

A detailed explanation about PRODIGY-lig can be found in published research articles:

- Vangone A, Schaarschmidt J, Koukos P, Geng C, Citro N, Trellet M, Xue L, Bonvin A.: [Large-scale prediction of binding affinity in protein-small ligand complexes: the PRODIGY-LIG web server.](https://doi.org/10.1093/bioinformatics/bty816) Bioinformatics

- Kurkcuoglu Z, Koukos P, Citro N, Trellet M, Rodrigues J, Moreira I, Roel-Touris J, Melquiond A, Geng C, Schaarschmidt J, Xue L, Vangone A, Bonvin AMJJ.: [Performance of HADDOCK and a simple contact-based protein-ligand binding affinity predictor in the D3R Grand Challenge 2.](https://link.springer.com/article/10.1007/s10822-017-0049-y) J Comput Aided Mol Des 32(1):175-185 (2017).


#### Notable parameters

The most important parameters for the `[prodigyligand]` module are:

- `receptor_chain`: Defines the chain ID of the receptor.

- `ligand_chain`: Defines the chain ID where the ligand/small-molecule is part of.

- `ligand_resname`: Defines the name of the residue in `ligand_chain` to be considered.

- `to_pkd`: Converts predicted binding affinity values to pKd values.


More information about `[prodigyligand]` parameters can be accessed [here](https://bonvinlab.org/haddock3/src/modules/scoring/haddock.modules.scoring.prodigyligand.html#default-parameters) or retrieved by running:

```bash
haddock3-cfg -m prodigyligand
```

<hr>

## `[deeprank]` module

Deep learning scoring of protein-protein interfaces with DeepRank-GNN-ESM.

This module scores the models with DeepRank-GNN-ESM ([GitHub](https://github.com/haddocking/DeepRank-GNN-esm), [PyPI package](https://pypi.org/project/deeprank-gnn-esm/)), a graph neural network that uses protein language model (ESM) embeddings to predict the fraction of native contacts (fnat) of an interface between two chains.
Every chain pair (interface) of a model is scored, and the models are ranked by their predicted fnat (between 0 and 1, larger is better).
As HADDOCK3 sorts models by increasing score, the score given to a model is minus its predicted fnat.

DeepRank-GNN-ESM is an optional dependency, installed with:

```bash
pip install 'haddock3[deeprank-gnn-esm]'
```

Only amino acids are used for the prediction: waters, ions and ligands are left out.

A detailed explanation about DeepRank-GNN-ESM can be found in:

- Xu X, Bonvin A.M.J.J.: DeepRank-GNN-esm: a graph neural network for scoring protein-protein models using protein language model. Bioinformatics Advances, 4, vbad191 (2024) ([10.1093/bioadv/vbad191](https://doi.org/10.1093/bioadv/vbad191))
- A detailed protocol: [arXiv:2407.16375](https://arxiv.org/abs/2407.16375)

#### Notable parameters

The most important parameters for the `[deeprank]` module are:

- `score_type`: predicted fnat used to rank the models:
  - `mean` (default): the mean predicted fnat over all interfaces of a model;
  - `min`: the lowest predicted fnat over all interfaces of a model;
  - `pair`: the predicted fnat of the interface between `chain_i` and `chain_j`.

  An interface without contacts has no prediction. It is left out of `mean` and `min`, and a model without any interface gets a fnat of 0. With `pair`, such an interface counts as 0.
- `chain_i` and `chain_j`: chain IDs of the interface used when `score_type = "pair"` (default: `A` and `B`).

#### Output files

Besides the usual `deeprank.tsv` file with the scores of the models, the module writes two files in its step folder:

- `GNN_esm_prediction.csv`: the predicted fnat of every interface (`pdb_id,chain_i,chain_j,predicted_fnat`), with `NA` for an interface without contacts;
- `GNN_esm_prediction_summary.csv`: per model, the number of scored interfaces and the mean, standard deviation and minimum of their predicted fnat (`pdb_id,scored_pairs,mean_fnat,std_fnat,min_fnat`).

More information about `[deeprank]` parameters can be retrieved by running:

```bash
haddock3-cfg -m deeprank
```

<hr>
