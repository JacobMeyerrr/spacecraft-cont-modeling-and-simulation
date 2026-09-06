# Figure Index and Provenance

This index maps the presentation-ready README assets to the original research figures. The molecular-dynamics source files came from `SPIE_Paper_Final_Version.zip`; filenames below preserve the exact capitalization and spelling found in that archive.

All three-view groups follow the LaTeX paper's intended display proportions: **30% top / 30% side / 22% perspective**, with approximately 2% spacing between panels.

## System-Scale and QCM Figures

| README asset | Content | Source |
|---|---|---|
| `assets/system/ctsp-usc-transport-fields.png` | CTSP surface deposition and molecular-number-density fields for the USC geometry | Brieda et al. (2022), Fig. 4 |
| `assets/system/ctsp-blue-origin-transport-fields.png` | CTSP result for the Blue Origin chamber | Brieda et al. (2022), Fig. 8 |
| `assets/system/blue-origin-vacuum-chamber.png` | Physical Blue Origin test configuration | Brieda et al. (2022) |
| `assets/qcm/multigaussian-qtga-deconvolution.png` | Two-, three-, and four-Gaussian QTGA fits | Follow-on multispecies manuscript |
| `assets/qcm/hot-to-cold-validation.png` | Six-Gaussian and activation-energy validation results | Follow-on multispecies manuscript, Table III |

## Atomistic Hero

| README asset suffix | Original filename |
|---|---|
| `final-heterogeneous-top.webp` | `Het350_top.png` |
| `final-heterogeneous-side.webp` | `Het350_Side.png` |
| `final-heterogeneous-perspective.webp` | `Het350_Persp.png` |

This triplet matches the extended heterogeneous-deposition morphology shown as the two-times-duration result in the published conference paper.

## Deposition Sequences

### Sequential sandwich case

| README state | Original top | Original side | Original perspective |
|---|---|---|---|
| `00-initial` | `InitialDepositionSetup_BothCases_Top.png` | `AllDepositionCases_Initial_Side.png` | `InitialDepositionCases_All_Perspective.png` |
| `01-water-layer` | `SandDeposition_WaterLayer_top.png` | `SandDeposition_WaterLayer_Side.png` | `SandDeposition_WaterLayer_perspective.png` |
| `02-hydrocarbon-layer` | `SandDeposition_AfterHydrocarbonLayer_Top.png` | `SandDeposition_AfterHydrocarbonLayer_Side.png` | `SandDeposition_AfterHydrocarbonLayer_Perspective.png` |
| `03-final-water-layer` | `SandDeposition_End_Top.png` | `SandDeposition_End_Side.png` | `SandDeposition_End_Perspective.png` |

### Heterogeneous case

| README state | Original top | Original side | Original perspective |
|---|---|---|---|
| `00-initial` | `InitialDepositionSetup_BothCases_Top.png` | `AllDepositionCases_Initial_Side.png` | `InitialDepositionCases_All_Perspective.png` |
| `01-timestep-2000000` | `HeteroDeposition_TS2Mil_Top.png` | `HeteroDeposition_TS2MIL_Side.png` | `HeteroDeposition_2Mil_Perspective.png` |
| `02-final` | `HeteroDeposition_EndCase_Top.png` | `HeteroDeposition_EndCase_Side.png` | `HeteroDeposition_EndCase_Perspective.png` |
| `03-extended-final` | `Het350_top.png` | `Het350_Side.png` | `Het350_Persp.png` |

## Corrected-Toluene 350 K Desorption Sequences

### Sequential sandwich case

| README state | Original top | Original side | Original perspective |
|---|---|---|---|
| `00-initial` | `Sand350_TS0_Top.png` | `Sand350_TS0_Side.png` | `Sand350_TS0_Perspective.png` |
| `01-timestep-372000` | `Sand350_TS373_Top.png` | `Sand350_TS373_Side.png` | `Sand350_TS372_Perspectibe.png` |
| `02-timestep-534000` | `Sand350_TS534_Top.png` | `Sand350_TS534_Side.png` | `Sand350_TS534_Perspective.png` |
| `03-timestep-711000` | `Sand350_TS711_Top.png` | `Sand350_TS711_Side.png` | `Sand350_TS711_Perspective.png` |

The source names disagree by 1,000 steps for the second state (`372` versus `373`). The paper captions the state as 372,000, so the README uses **approximately 372,000**.

### Heterogeneous case

| README state | Original top | Original side | Original perspective |
|---|---|---|---|
| `00-input-film` | `Het350_top.png` | `Het350_Side.png` | `Het350_Persp.png` |
| `01-timestep-138000` | `Het350_TS138_Top.png` | `Het350_TS138_Side.png` | `Het350_TS138_Perspective.png` |
| `02-timestep-498000` | `Het350_TS498_top.png` | `Het350_TS498_side.png` | `Het350_TS498_Perspective.png` |
| `03-timestep-1152000` | `Het350_TS1152_Top.png` | `Het350_TS1152_Side.png` | `Het350_TS1152_Perspective.png` |

These are the three 350 K heterogeneous states shown in the published paper, preceded here by their full-film initial condition. The archive also contains a 350,000-step triplet (`Het350_top2.png`, `Het350_side2.png`, and `Het350_pers2.png`) whose LaTeX block is commented out; it is retained under `assets/md/desorption/heterogeneous-350k/supplemental-draft-state/` but omitted from the publication-matched README sequence.

## Quantitative and Methods Figures

| README asset | Original/source |
|---|---|
| `assets/md/desorption/sandwich-350k/molecule-counts.png` | `350kSandMolCount.png` |
| `assets/md/desorption/heterogeneous-350k/molecule-counts.png` | `350kHeteroMolCount.png` |
| `assets/md/methods/deposition-interval-sensitivity.png` | Published-paper composite of the 1k, 5k, and 10k side-view cases |
| `assets/md/methods/molecular-models-force-fields.png` | Published-paper molecule/force-field table |

## Interpretation Notes

- Yellow spheres depict Au substrate atoms.
- Molecular-render colors primarily encode atom types and geometry. They should not be interpreted as a scalar field or a unique one-color-per-species legend.
- PEG-600 is present in the parameter table as an ancillary model, but it is not identified as part of the primary five-species sandwich and heterogeneous trajectories shown in the README.
- The archived 450 K `Sandwich_TS*` and `HETERO_TS*` series used an erroneous, overly strong toluene-hydrogen Lennard-Jones parameterization. Those images are intentionally excluded from the primary README result sequence.
