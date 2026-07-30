# List of supported particles
Haddock3 supports (i.e., can process automatically including topology generation, rebuilding of the missing atoms, and point mutations) various carbohydrates, ions, co-factors and a plethora of the modified amino acids - you can check a complete list below.

### Supported carbohydrates
* A2G: 2-N-acetyl-alpha-D-glucopyranose, different stereochemistry at C4
* ABE: alpha-D-Abequopyranose
* BDP: beta-D-glucoronic acid
* BGC: beta-D-glucopyranose
* BMA: beta-D-mannopyronose
* FCA: alpha-D-fucopyranose
* FCB: beta-D-fucopyranose
* FUC: alpha-L-fucopyranose
* FUL: beta-L-fucopyranose
* GAL (GLB): beta-D-galactopyranose
* GLA: alpha-D-galactopyranose
* GLC: alpha-D-glucopyranose
* GCS: 2-N-beta-D-glucopyranose (glucosamine)
* GXL: alpha-L-galactopyranose
* MAG: 2-N-Acetyl-beta-D-glucopyranose + Methyl at O1
* MAN: alpha-D-mannopyranose
* MMA: methyl alpha-D-mannopyranoside
* NAA: 1-O-methyl-2-N-Acetyl-alpha-D-glucopyranose
* NAG: 2-N-acetyl-beta-D-glucopyranose
* NAM: 1-O-methyl-2-N-Acetyl-beta-D-glucopyranose
* NDG: 2-N-acetyl-alpha-D-glucopyranose
* NGA: 2-N-acetyl-beta-D-galactopyranose
* NGM: 1-O-methyl-2-N-Acetyl-alpha-D-galactopyranose
* RAM: alpha-L-rhampyranose
* SIA: alpha-N-acetyl neuraminic acid
* SIB: beta-N-acetyl neuraminic acid
* XYP: beta-D-xylopyranose
* XYS: alpha-D-xylopyranose

Carbohydrates should be provided with record **HETAM**, e.g.:
```
HETATM    9  C1  NAG B 102     171.992  99.750 236.168  1.00 10.00           C
```

### Supported ions
| | | |
| --- | --- | --- |
| AG: Silver | AL: Aluminium | AU: Gold |
| BR: Bromine | CA: Calcium | CD: Cadmium |
| CL: Chlore | CO: Cobalt | CR: Chromium |
| CS: Cesium | CU: Copper | F: Fluor |
| FE: Iron | HG: Mercury | HO: Holmium |
| I: Iodine | IR: IridiumP | K: Potassium |
| KR: Krypton | LI: Adenine | MG: Magnesium |
| MN: Manganese | MO: Molybdenum | NA: Sodium |
| NI: Nickel | OS: Osmium | PB: Lead |
| PT: Platinum | SR: Strontium | U: Uranium |
| V: Vanadium | YB: Ytterbium | ZN: Zinc |

Ions should be provided with their charge and a **HETAM** flag. Residue name should contain the absolute charge (e.g. ZN2) while atom name should have the exact charge (e.g. ZN+2), e.g.:
```
HETATM 3833 ZN+2 ZN2 A  42      21.391  -8.794  33.944  1.00 24.37          ZN  
```

## Supported multi-atom ions 
* PO4: Phosphate
* SO4: Sulphate
* WO4: Tungstate

## Supported water molecules
* TIP/WAT: water molecule with as atoms OH2,H1,H2

## Supported co-factors
The following co-factors are defined using HADDOCK's internal parameters:
* HEB: Heme B, [open pdb example](https://raw.githubusercontent.com/haddocking/haddock3/refs/heads/main/src/haddock/cns/toppar/hemeB.pdb)
* HEC: Heme C, [open pdb example](https://raw.githubusercontent.com/haddocking/haddock3/refs/heads/main/src/haddock/cns/toppar/heme.pdb)

Co-factors should be provided with record **HETAM**, e.g.:
```
HETATM    1  FE  HEB   500       3.565   0.487   0.949  1.00 15.00          
```
For the other co-factors, HADDOCK3 will try to generate the topology and parameters automatically, using the PRODRG server - do add `autotoppar = true` to the `[topoaa]` to enable this option. 
If the co-factor is not available in PRODRG, you will have to [generate and provide the topology and parameters yourself](../structure_requirements.md#dealing-with-non-standard-molecules).

## Supported nucleic acid bases
| DNA | RNA |
| --- | --- |
| DA: Adenine | A: Adenine |
| DC: Cytosine | C: Cytosine |
| DG: Guanine | G: Guanine |
| DT: Thymidine | U: Uridine |

## Supported modified amino acids
All supported modified amino acids are in this list and must be defined as **ATOM**. 

* **ACE**: N-terminal acetyl group.

    Atoms: CA,HA1,HA2,HA3,C,O
* **ALY**: Acetylated LYS.

    Atoms: N,HN,CA,HA,CB,HB1,HB2,CG,HG1,HG2,CD,HD1,HD2,CE1,HE1,HE2,NZ,HZ1,CZ,OZ,CM,HM1,HM2,HM3,C,O
* **ASH**: protonated ASP.

    Atoms: N,HN,CA,HA,CB,HB1,HB2,CG,OD1,OD2,HD2,C,O
* **CFE**: CYS with an iron sulfur cluster. HADDOCK will automatically search for closeby CYF residues and created bonds with the iron cluster.

    Atoms: N,HN,CA,HA,CB,HB1,HB2,SG,FE1,SF1,FE2,SF2,C,O
* **CSP**: phosphorylated CYS.

    Atoms: N,HN,CA,HA,CB,HB1,HB2,SG,P,O1P,O2P,O3P,C,O
* **CTN**: C-terminal amide group.

    Atoms: N,H1,H2
* **CYC**: special for covalent docking - vdw of Sulphur reduced.

    Atoms: N,HN,CA,HA,CB,HB1,HB2,SG,C,O
* **CYF**: CYS without the sulfur H (for coordinating metals).

    Atoms: N,HN,CA,HA,CB,HB1,HB2,SG,C,O
* **CYM**: CYS with MTSL grouped.

    Atoms: CAC,CAS,CAD,NAQ,OAH,HAA,CAR,CAA,CAB,CAI,CAO,CAJ,SAL,N,HN,CA,HA,CB,HB1,HB2,SG,C,O
* **DDZ**: 3,3,-dihydroxy ALA.

    Atoms: N,HN,CA,HA,CB,HB1,OG1,HG1,OG2,HG2
* **GLH**: protonated GLU.

    Atoms: N,HN,CA,HA,CB,HB1,HB2,CG,HG1,HG2,CD,OE1,OE2,HE2,C,O
* **HY3**: 3-hydroxyproline.

    Atoms: N,CA,HA,CB,OB1,HB1,HB2,CG,HG1,HG2,CD,HD1,HD2,C,O
* **HYP**: 4R-hydroxyproline.

    Atoms: N,CA,HA,CB,HB1,HB2,CG,OG1,HG1,HG2,CD,HD1,HD2,C,O
* **M3L**: trimethyl LYS.

    Atoms: N,HN,CA,HA,CB,HB1,HB2,CG,HG1,HG2,CD,HD1,HD2,CE1,HE1,HE2,NZ,HZ1,HZ2,CM1,HM11,HM12,HM13,CM2,HM21,HM22,HM23,CM3,HM31,HM32,HM33,C,O
* **MLY**: dimethyl LYS.

    Atoms: N,HN,CA,HA,CB,HB1,HB2,CG,HG1,HG2,CD,HD1,HD2,CE1,HE1,HE2,NZ,HZ1,HZ2,CM2,HM21,HM22,HM23,CM3,HM31,HM32,HM33,C,O
* **MLZ**: monomethyl LYS.

    Atoms: N,HN,CA,HA,CB,HB1,HB2,CG,HG1,HG2,CD,HD1,HD2,CE1,HE1,HE2,NZ,HZ1,HZ2,CM3,HM31,HM32,HM33,C,O
* **MSE**: Selenomethionine.

    Atoms: N,HN,CA,HA,CB,HB1,HB2,CG,HG1,HG2,SE,CE,HE1,HE2,HE3,C,O
* **NEP**: NE phosphorylated HIS.

    Atoms: N,HN,CA,HA,CB,HB1,HB2,CG,ND1,HD1,CD2,HD2,CE1,HE1,NE2,P,O1P,O2P,O3P,C,O
* **NME**: C-terminal N-Methyl.

    Atoms: N,HN,CA,HA1,HA2,HA3
* **PNS**: Phosphopanthetine Serine.

    Atoms: N,HN,CA,HA,CB,HB1,HB2,OG,P,O1P,O2P,O3P,C,O,C28,H28,H29,C29,C30,H31,H32,H30,C31,H34,H35,H33,C32,H36,O33,H46,C34,O35,N36,H48,C37,H37,H38,C38,H39,H4A,C39,O40,N41,H41,C42,H42,H43,C43,H44,H45,S44,H47
* **PTR**: O-Phosphotyrosine.

    Atoms: N,CA,C,O,OXT,CB,CG,CD1,CD2,CE1,CE2,CZ,OH,P,O1P,O2P,O3P,1HN,2HN,HA,HXT,1HB,2HB,HD1,HD2,HE1,HE2,PHO2,PHO3
* **SEC**: Serine with reduced vdw on the OG oxygen for covalent docking.

    Atoms: N,HN,CA,HA,CB,HB1,HB2,OG,C,O
* **SEP**: phosphorylated SER.

    Atoms: N,HN,CA,HA,CB,HB1,HB2,OG,P,O1P,O2P,O3P,C,O
* **TOP**: phosphorylated THR.

    Atoms: N,HN,CA,HA,CB,HB1,HB2,OG,P,O1P,O2P,O3P,CG2,HG21,HG22,HG23,C,O
* **TYP** or **PTR**: phosphorylated TYR.

    Atoms: N,HN,CA,HA,CB,HB1,HB2,CG,CD1,HD1,CD2,HD2,CE1,HE1,CE2,HE2,CZ,OH,P,O1P,O2P,O3P,C,O
* **TYS**: sulfonated TYR.

    Atoms: N,HN,CA,HA,CB,HB1,HB2,CG,CD1,HD1,CD2,HD2,CE1,HE1,CE2,HE2,CZ,OH,S,O1S,O2S,O3S,C,O
* **QSR**: serotonylated GLN.

    Atoms: N,HN,CA,HA,CB,HB1,HB2,CG,HG1,HG2,CD,OE1,NE2,HE2,CSA,HSA1,HSA2,CSB,HSB1,HSB2,HSE1,NSE1,CSD1,HSD1,CSG,CSD2,CSE2,CSE3,HSE3,CSZ3,OSZ3,HSZ3,CSH2,HSH2,CSZ2,HSZ2,C,O,
* **PCA**: pyro-glutamic acid.

    Atoms: N,HN,CA,HA,CB,HB1,HB2,CG,HG1,HG2,CD,OE,C,O