# Spacecraft Contamination Modeling & Simulation

**Multiscale research connecting experimentally informed, spacecraft-scale molecular transport with atomistic molecular dynamics of heterogeneous contaminant films.**

[![SPIE DOI](https://img.shields.io/badge/SPIE-10.1117%2F12.3066473-0b6cb8)](https://doi.org/10.1117/12.3066473)

[Overview](#overview) · [Decisions](#engineering-decisions) · [Architecture](#multiscale-architecture) · [Validation](#engineering-scale-transport-and-validation) · [Deposition](#deposition) · [Desorption](#desorption) · [Reproducibility](#historical-execution-environment-and-reproducibility) · [Publications](#publications)

> **My direct contributions:** QCM signal decomposition, CTSP experimental-correlation studies, GPU LAMMPS workflow development and execution, sensitivity analysis, scientific post-processing, and first-author publication. The resulting multispecies cases reached **4.3%** and **5.5%** error, while the atomistic study exposed deposition-history and material-pair effects missing from reduced-order contamination models.

<p align="center">
  <a href="assets/md/hero/final-heterogeneous-top.webp"><img src="assets/md/hero/final-heterogeneous-top.webp" width="30%" alt="Top view of the extended heterogeneous contaminant film on gold"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/hero/final-heterogeneous-side.webp"><img src="assets/md/hero/final-heterogeneous-side.webp" width="30%" alt="Side view of the extended heterogeneous contaminant film on gold"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/hero/final-heterogeneous-perspective.webp"><img src="assets/md/hero/final-heterogeneous-perspective.webp" width="22%" alt="Perspective view of the extended heterogeneous contaminant film on gold"></a>
</p>

<p align="center"><sub><strong>Atomistic scale:</strong> top, side, and perspective views of the extended heterogeneous-deposition case, exposing lateral coverage, interface-normal structure, clustering, and three-dimensional morphology. Source: Meyer, Brieda, and Wang (2025).</sub></p>

## Overview

This repository documents nearly two years of spacecraft-contamination modeling and simulation across two complementary scales. At the engineering scale, quartz crystal microbalance thermogravimetric analysis (QTGA) data were decomposed into virtual contaminant species and propagated through the Contamination Transport Simulation Program (CTSP) to model multiple-bounce, non-line-of-sight transport. At the atomistic scale, classical non-reactive LAMMPS simulations resolved the deposition, restructuring, re-adsorption, and thermally activated desorption of mixed molecular films on gold.

The best reported multispecies cases reduced experimental-correlation error to **4.3%** for the four-Gaussian warm-wall case and **5.5%** for the six-Gaussian hot-to-cold case. The molecular-dynamics results independently showed that film morphology and desorption depend on deposition history, molecular interactions, and the specific adsorbate-substrate pairing.

The long-term objective is **atomistically informed model reduction**: converting those interfacial-physics observations into species- and material-pair-dependent surface-interaction closures for higher-fidelity spacecraft-scale transport models. That coupling is future work; this repository does not claim that the LAMMPS results have already been calibrated into CTSP.

## Results at a Glance

| Scale | Capability or objective | Evidence in this repository |
|---|---|---|
| System / chamber | Three-dimensional, multiple-bounce molecular transport through complex geometry | CTSP particle tracing, view factors, deposited-film fields, and non-line-of-sight QCM comparisons |
| Experimental correlation | QCM-derived multispecies parameterization | 4.3% warm-wall and 5.5% hot-to-cold errors in the best reported Gaussian cases |
| Atomistic / interface | Mixed-film deposition and thermal evolution on Au | Sequential and heterogeneous LAMMPS trajectories with top, side, and perspective state histories |
| Future cross-scale integration | Identification of physics that could inform reduced-order closures | Dynamic restructuring, re-adsorption, clustering, and material-pair-dependent persistence |

## My Contributions

- Developed and evaluated the multi-Gaussian virtual-species workflow used to derive relative mass fractions and temperature-dependent sticking behavior from QTGA measurements.
- Configured CTSP transport cases and compared predicted QCM deposition ratios against experimental data.
- Built and ran LAMMPS deposition and desorption cases, including rigid-molecule insertion workarounds, force-field integration, thermal ramps, restart handling, and SLURM/Apptainer execution on USC high-performance-computing resources.
- Designed sensitivity studies, post-processed trajectories, produced scientific visualizations, interpreted cross-scale behavior, and led the accompanying publications as first author.

## Engineering Decisions

| Decision | Rationale | Verification / evidence |
|---|---|---|
| Replace one unitary sticking curve with empirical virtual species | Preserve multimodal QTGA behavior without claiming unsupported chemical identities | QCM2 correlation improved to 4.3% and 5.5% error in the best reported cases |
| Compare sequential and simultaneous deposition histories | Isolate how initial morphology and underlying material alter later desorption | Full sandwich and heterogeneous state histories below |
| Use TIP4P/Ice, OPLS-AA, EAM Au, and a benchmarked water-Au Morse interaction | Represent cryogenic mixed films and the Au interface within a classical non-reactive model | Force-field definitions and sensitivity results are archived with the study |
| Feature the corrected-toluene 350 K runs | Keep the headline results physically interpretable and separate from a known early parameter error | Corrected species histories and clean/persistent endpoints shown below |
| Run containerized GPU jobs with restart checkpoints | Support long HPC trajectories and recovery across scheduler limits | Historical launch pattern, restart workflow, and release checklist are documented |

## Multiscale Architecture

```mermaid
flowchart TD
    A["QTGA measurements"] --> B["Gaussian virtual-species model"]
    B --> C["CTSP particle tracing and view factors"]
    C --> D["System-scale deposition predictions"]
    E["LAMMPS interfacial simulations"] --> F["Resolved surface physics"]
    F -. "future reduced-order closures" .-> C
```

Solid arrows indicate implemented workflows. The dashed arrow identifies the planned cross-scale coupling.

## Engineering-Scale Transport and Validation

### QTGA to virtual contaminant species

Each Gaussian component represents an empirical **virtual species**, not a claimed one-to-one chemical identification. Its area supplies a relative outgassing mass fraction, while its cumulative distribution supplies temperature-dependent sticking behavior.

<p align="center">
  <a href="assets/qcm/multigaussian-qtga-deconvolution.png"><img src="assets/qcm/multigaussian-qtga-deconvolution.png" width="78%" alt="Two-, three-, and four-Gaussian decompositions of a QCM thermogravimetric-analysis signal"></a>
</p>

<p align="center"><sub>Two-, three-, and four-Gaussian decompositions of an experimental QTGA signal.</sub></p>

### Three-dimensional molecular transport

CTSP propagates virtual contaminant species through chamber geometry using particle tracing and view-factor transport. This represents repeated surface interactions and deposition on sensors without direct line of sight to the source.

<p align="center">
  <a href="assets/system/ctsp-usc-transport-fields.png"><img src="assets/system/ctsp-usc-transport-fields.png" width="94%" alt="CTSP model of the USC vacuum chamber, with deposited-film thickness on surfaces and a molecular-number-density field through the chamber"></a>
</p>

<p align="center"><sub>CTSP simulation of the USC chamber. Orange/brown surface shading shows deposited-film thickness; blue → green → yellow → red/pink shows increasing molecular number density. Green is an intermediate density, not a molecular species. Source: Brieda et al. (2022).</sub></p>

<p align="center">
  <a href="assets/system/ctsp-blue-origin-transport-fields.png"><img src="assets/system/ctsp-blue-origin-transport-fields.png" width="82%" alt="CTSP model of the Blue Origin chamber with molecular-number-density and deposited-film-thickness fields"></a>
</p>

<p align="center"><sub>CTSP prediction for the Blue Origin chamber configuration. Source: Brieda et al. (2022).</sub></p>

<p align="center">
  <a href="assets/system/blue-origin-vacuum-chamber.png"><img src="assets/system/blue-origin-vacuum-chamber.png" width="58%" alt="Blue Origin vacuum chamber with a heated outgassing sample and QCM instrumentation"></a>
</p>

<p align="center"><sub>Physical test configuration used to generate the system-scale validation data. Source: Brieda et al. (2022).</sub></p>

### Experimental correlation

<p align="center">
  <a href="assets/qcm/hot-to-cold-validation.png"><img src="assets/qcm/hot-to-cold-validation.png" width="78%" alt="Hot-to-cold QCM validation table showing 5.5 percent error for the six-Gaussian sticking-coefficient model"></a>
</p>

<p align="center"><sub>The six-Gaussian sticking-coefficient model reached 5.5% error; the two evaluated Arrhenius activation-energy fits produced 620% and 2502% error.</sub></p>

## Atomistic Molecular Dynamics

The primary production cases contain water, methane, nitrogen, decane, and toluene on a gold substrate. The simulations use lateral periodicity, explicit force fields, long-range electrostatics, constrained rigid molecules, and open/evaporative behavior above the surface during desorption.

| Model element | Implementation |
|---|---|
| Engine | LAMMPS; classical, non-reactive molecular dynamics |
| Deposition conditions | 273.15 K incident thermal velocities; 73.15 K Au substrate |
| Desorption sequence shown below | Corrected-toluene 350 K trajectory |
| Water / electrostatics | TIP4P/Ice with `pppm/tip4p`, relative RMS force tolerance `1e-5` |
| Organic molecules | OPLS-AA representations for methane, decane, and toluene |
| Gold and interface | EAM Au; Morse water-Au interaction; Lennard-Jones cross interactions |
| Dynamics | Velocity-Verlet/NVE propagation; SHAKE constraints; thermostatted Au during deposition and Berendsen thermal ramping for desorption |
| Nominal timestep | 1.54547 fs |
| Execution / visualization | SLURM + Apptainer on USC HPC resources; OVITO and ParaView |

<p align="center">
  <a href="assets/md/methods/molecular-models-force-fields.png"><img src="assets/md/methods/molecular-models-force-fields.png" width="92%" alt="Table of molecular species, visual representations, force-field families, and nonbonded parameters used in the molecular-dynamics model"></a>
</p>

<p align="center"><sub>Molecular models and force-field definitions. PEG-600/CHARMM36 is documented as an ancillary parameterization; the primary sequences below use water, methane, nitrogen, decane, and toluene.</sub></p>

## Deposition

Each state is shown **top / side / perspective**, left to right, at the paper's original `30% / 30% / 22%` proportions. Yellow atoms are the Au substrate. The remaining colors primarily encode atom types and molecular geometry, not a scalar field or a one-color-per-species legend; use the molecular-model table above to identify species.

### Sequential “Sandwich” Deposition

The sandwich case isolates deposition history by forming a water layer, adding the mixed contaminant layer, and finishing with a second water layer.

**State 0 - initial injection configuration**

<p align="center">
  <a href="assets/md/deposition/sandwich/00-initial-top.webp"><img src="assets/md/deposition/sandwich/00-initial-top.webp" width="30%" alt="Top view of the initial sandwich deposition configuration"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/deposition/sandwich/00-initial-side.webp"><img src="assets/md/deposition/sandwich/00-initial-side.webp" width="30%" alt="Side view of the initial sandwich deposition configuration"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/deposition/sandwich/00-initial-perspective.webp"><img src="assets/md/deposition/sandwich/00-initial-perspective.webp" width="22%" alt="Perspective view of the initial sandwich deposition configuration"></a>
</p>

<p align="center"><sub>A molecule is introduced above the clean 73 K Au surface.</sub></p>

**State 1 - first water layer**

<p align="center">
  <a href="assets/md/deposition/sandwich/01-water-layer-top.webp"><img src="assets/md/deposition/sandwich/01-water-layer-top.webp" width="30%" alt="Top view after the first water layer formed in the sandwich deposition case"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/deposition/sandwich/01-water-layer-side.webp"><img src="assets/md/deposition/sandwich/01-water-layer-side.webp" width="30%" alt="Side view after the first water layer formed in the sandwich deposition case"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/deposition/sandwich/01-water-layer-perspective.webp"><img src="assets/md/deposition/sandwich/01-water-layer-perspective.webp" width="22%" alt="Perspective view after the first water layer formed in the sandwich deposition case"></a>
</p>

<p align="center"><sub>The initial water film forms directly on Au.</sub></p>

**State 2 - mixed contaminant layer**

<p align="center">
  <a href="assets/md/deposition/sandwich/02-hydrocarbon-layer-top.webp"><img src="assets/md/deposition/sandwich/02-hydrocarbon-layer-top.webp" width="30%" alt="Top view after the mixed contaminant layer formed in the sandwich case"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/deposition/sandwich/02-hydrocarbon-layer-side.webp"><img src="assets/md/deposition/sandwich/02-hydrocarbon-layer-side.webp" width="30%" alt="Side view after the mixed contaminant layer formed over water in the sandwich case"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/deposition/sandwich/02-hydrocarbon-layer-perspective.webp"><img src="assets/md/deposition/sandwich/02-hydrocarbon-layer-perspective.webp" width="22%" alt="Perspective view after the mixed contaminant layer formed in the sandwich case"></a>
</p>

<p align="center"><sub>Methane, N₂, decane, and toluene form the intermediate layer over water.</sub></p>

**State 3 - completed layered film**

<p align="center">
  <a href="assets/md/deposition/sandwich/03-final-water-layer-top.webp"><img src="assets/md/deposition/sandwich/03-final-water-layer-top.webp" width="30%" alt="Top view of the final sandwich deposition film after the second water layer formed"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/deposition/sandwich/03-final-water-layer-side.webp"><img src="assets/md/deposition/sandwich/03-final-water-layer-side.webp" width="30%" alt="Side view of the final sandwich deposition film after the second water layer formed"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/deposition/sandwich/03-final-water-layer-perspective.webp"><img src="assets/md/deposition/sandwich/03-final-water-layer-perspective.webp" width="22%" alt="Perspective view of the final sandwich deposition film after the second water layer formed"></a>
</p>

<p align="center"><sub>The final water deposition completes the water / mixed-contaminant / water sequence.</sub></p>

The first two layers remain comparatively stratified, while the final water deposition clusters and partially penetrates the rough mixed layer. That difference motivated the direct comparison with simultaneous heterogeneous deposition.

### Heterogeneous Deposition

The heterogeneous case samples water, methane, nitrogen, decane, and toluene throughout one simultaneous deposition history.

**State 0 - initial injection configuration**

<p align="center">
  <a href="assets/md/deposition/heterogeneous/00-initial-top.webp"><img src="assets/md/deposition/heterogeneous/00-initial-top.webp" width="30%" alt="Top view of the initial heterogeneous deposition configuration"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/deposition/heterogeneous/00-initial-side.webp"><img src="assets/md/deposition/heterogeneous/00-initial-side.webp" width="30%" alt="Side view of the initial heterogeneous deposition configuration"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/deposition/heterogeneous/00-initial-perspective.webp"><img src="assets/md/deposition/heterogeneous/00-initial-perspective.webp" width="22%" alt="Perspective view of the initial heterogeneous deposition configuration"></a>
</p>

<p align="center"><sub>The heterogeneous and sandwich cases begin from the same clean Au geometry.</sub></p>

**State 1 - timestep 2,000,000 (~3.09 ns)**

<p align="center">
  <a href="assets/md/deposition/heterogeneous/01-timestep-2000000-top.webp"><img src="assets/md/deposition/heterogeneous/01-timestep-2000000-top.webp" width="30%" alt="Top view of heterogeneous deposition at two million timesteps"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/deposition/heterogeneous/01-timestep-2000000-side.webp"><img src="assets/md/deposition/heterogeneous/01-timestep-2000000-side.webp" width="30%" alt="Side view of heterogeneous deposition at two million timesteps"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/deposition/heterogeneous/01-timestep-2000000-perspective.webp"><img src="assets/md/deposition/heterogeneous/01-timestep-2000000-perspective.webp" width="22%" alt="Perspective view of heterogeneous deposition at two million timesteps"></a>
</p>

<p align="center"><sub>The first heterogeneous monolayer is nearly complete.</sub></p>

**State 2 - end of the original deposition run**

<p align="center">
  <a href="assets/md/deposition/heterogeneous/02-final-top.webp"><img src="assets/md/deposition/heterogeneous/02-final-top.webp" width="30%" alt="Top view at the end of the original heterogeneous deposition run"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/deposition/heterogeneous/02-final-side.webp"><img src="assets/md/deposition/heterogeneous/02-final-side.webp" width="30%" alt="Side view at the end of the original heterogeneous deposition run"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/deposition/heterogeneous/02-final-perspective.webp"><img src="assets/md/deposition/heterogeneous/02-final-perspective.webp" width="22%" alt="Perspective view at the end of the original heterogeneous deposition run"></a>
</p>

<p align="center"><sub>The mixed film develops nonuniform coverage, roughness, and molecular clustering.</sub></p>

**State 3 - extended run (2× the original simulation duration)**

<p align="center">
  <a href="assets/md/deposition/heterogeneous/03-extended-final-top.webp"><img src="assets/md/deposition/heterogeneous/03-extended-final-top.webp" width="30%" alt="Top view of the extended heterogeneous deposition run"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/deposition/heterogeneous/03-extended-final-side.webp"><img src="assets/md/deposition/heterogeneous/03-extended-final-side.webp" width="30%" alt="Side view of the extended heterogeneous deposition run"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/deposition/heterogeneous/03-extended-final-perspective.webp"><img src="assets/md/deposition/heterogeneous/03-extended-final-perspective.webp" width="22%" alt="Perspective view of the extended heterogeneous deposition run"></a>
</p>

<p align="center"><sub>Extended deposition produces the complete mixed film used to initialize the thermal studies.</sub></p>

Unlike the prescribed sandwich history, simultaneous deposition produces a laterally heterogeneous morphology in which direct Au contact varies by molecule. That difference becomes important during desorption.

### Deposition-interval sensitivity

<p align="center">
  <a href="assets/md/methods/deposition-interval-sensitivity.png"><img src="assets/md/methods/deposition-interval-sensitivity.png" width="94%" alt="Water deposition sensitivity study comparing 1000-, 5000-, and 10000-timestep insertion intervals"></a>
</p>

<p align="center"><sub>Water-only sensitivity study at 1,000, 5,000, and 10,000 timesteps between insertions. The study exposed the trade between equilibration fidelity, solver stability, and practical HPC runtime.</sub></p>

## Desorption

The primary histories below use the **corrected toluene parameters** at 350 K. Every saved state again uses the paper's top / side / perspective proportions. The earlier 450 K exploratory sequence with the known excessively strong toluene-hydrogen Lennard-Jones parameters is intentionally excluded from headline results.

### Sequential “Sandwich” Desorption - 350 K

**State 0 - completed film before heating**

<p align="center">
  <a href="assets/md/desorption/sandwich-350k/00-initial-top.webp"><img src="assets/md/desorption/sandwich-350k/00-initial-top.webp" width="30%" alt="Top view of the complete sandwich film before the 350 kelvin desorption run"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/desorption/sandwich-350k/00-initial-side.webp"><img src="assets/md/desorption/sandwich-350k/00-initial-side.webp" width="30%" alt="Side view of the complete sandwich film before the 350 kelvin desorption run"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/desorption/sandwich-350k/00-initial-perspective.webp"><img src="assets/md/desorption/sandwich-350k/00-initial-perspective.webp" width="22%" alt="Perspective view of the complete sandwich film before the 350 kelvin desorption run"></a>
</p>

<p align="center"><sub>The fully deposited layered film is the thermal-run initial condition.</sub></p>

**State 1 - ~372,000 timesteps (~0.575 ns)**

<p align="center">
  <a href="assets/md/desorption/sandwich-350k/01-timestep-372000-top.webp"><img src="assets/md/desorption/sandwich-350k/01-timestep-372000-top.webp" width="30%" alt="Top view of sandwich desorption near 372000 timesteps"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/desorption/sandwich-350k/01-timestep-372000-side.webp"><img src="assets/md/desorption/sandwich-350k/01-timestep-372000-side.webp" width="30%" alt="Side view of sandwich desorption near 372000 timesteps"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/desorption/sandwich-350k/01-timestep-372000-perspective.webp"><img src="assets/md/desorption/sandwich-350k/01-timestep-372000-perspective.webp" width="22%" alt="Perspective view of sandwich desorption near 372000 timesteps"></a>
</p>

<p align="center"><sub>Early thermal relaxation thickens and restructures the initially layered film.</sub></p>

**State 2 - 534,000 timesteps (~0.825 ns)**

<p align="center">
  <a href="assets/md/desorption/sandwich-350k/02-timestep-534000-top.webp"><img src="assets/md/desorption/sandwich-350k/02-timestep-534000-top.webp" width="30%" alt="Top view of sandwich desorption at 534000 timesteps"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/desorption/sandwich-350k/02-timestep-534000-side.webp"><img src="assets/md/desorption/sandwich-350k/02-timestep-534000-side.webp" width="30%" alt="Side view of sandwich desorption at 534000 timesteps"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/desorption/sandwich-350k/02-timestep-534000-perspective.webp"><img src="assets/md/desorption/sandwich-350k/02-timestep-534000-perspective.webp" width="22%" alt="Perspective view of sandwich desorption at 534000 timesteps"></a>
</p>

<p align="center"><sub>Hydrocarbons enter the gas phase while water reorganizes against the Au surface.</sub></p>

**State 3 - 711,000 timesteps (~1.10 ns)**

<p align="center">
  <a href="assets/md/desorption/sandwich-350k/03-timestep-711000-top.webp"><img src="assets/md/desorption/sandwich-350k/03-timestep-711000-top.webp" width="30%" alt="Top view of the sandwich desorption end state at 711000 timesteps"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/desorption/sandwich-350k/03-timestep-711000-side.webp"><img src="assets/md/desorption/sandwich-350k/03-timestep-711000-side.webp" width="30%" alt="Side view of the sandwich desorption end state at 711000 timesteps"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/desorption/sandwich-350k/03-timestep-711000-perspective.webp"><img src="assets/md/desorption/sandwich-350k/03-timestep-711000-perspective.webp" width="22%" alt="Perspective view of the sandwich desorption end state at 711000 timesteps"></a>
</p>

<p align="center"><sub>The remaining water has desorbed, leaving a clean Au substrate.</sub></p>

### Heterogeneous Desorption - 350 K

**State 0 - complete heterogeneous input film**

<p align="center">
  <a href="assets/md/desorption/heterogeneous-350k/00-input-film-top.webp"><img src="assets/md/desorption/heterogeneous-350k/00-input-film-top.webp" width="30%" alt="Top view of the complete heterogeneous input film before desorption"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/desorption/heterogeneous-350k/00-input-film-side.webp"><img src="assets/md/desorption/heterogeneous-350k/00-input-film-side.webp" width="30%" alt="Side view of the complete heterogeneous input film before desorption"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/desorption/heterogeneous-350k/00-input-film-perspective.webp"><img src="assets/md/desorption/heterogeneous-350k/00-input-film-perspective.webp" width="22%" alt="Perspective view of the complete heterogeneous input film before desorption"></a>
</p>

<p align="center"><sub>The extended heterogeneous-deposition result supplies the full-film starting state.</sub></p>

**State 1 - 138,000 timesteps (~0.213 ns)**

<p align="center">
  <a href="assets/md/desorption/heterogeneous-350k/01-timestep-138000-top.webp"><img src="assets/md/desorption/heterogeneous-350k/01-timestep-138000-top.webp" width="30%" alt="Top view of heterogeneous desorption at 138000 timesteps"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/desorption/heterogeneous-350k/01-timestep-138000-side.webp"><img src="assets/md/desorption/heterogeneous-350k/01-timestep-138000-side.webp" width="30%" alt="Side view of heterogeneous desorption at 138000 timesteps"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/desorption/heterogeneous-350k/01-timestep-138000-perspective.webp"><img src="assets/md/desorption/heterogeneous-350k/01-timestep-138000-perspective.webp" width="22%" alt="Perspective view of heterogeneous desorption at 138000 timesteps"></a>
</p>

<p align="center"><sub>Early desorption begins with the weakest-bound population, including methane.</sub></p>

**State 2 - 498,000 timesteps (~0.770 ns)**

<p align="center">
  <a href="assets/md/desorption/heterogeneous-350k/02-timestep-498000-top.webp"><img src="assets/md/desorption/heterogeneous-350k/02-timestep-498000-top.webp" width="30%" alt="Top view of heterogeneous desorption at 498000 timesteps"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/desorption/heterogeneous-350k/02-timestep-498000-side.webp"><img src="assets/md/desorption/heterogeneous-350k/02-timestep-498000-side.webp" width="30%" alt="Side view of heterogeneous desorption at 498000 timesteps"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/desorption/heterogeneous-350k/02-timestep-498000-perspective.webp"><img src="assets/md/desorption/heterogeneous-350k/02-timestep-498000-perspective.webp" width="22%" alt="Perspective view of heterogeneous desorption at 498000 timesteps"></a>
</p>

<p align="center"><sub>Water clusters merge while some hydrocarbons remain in direct contact with Au.</sub></p>

**State 3 - 1,152,000 timesteps (~1.78 ns)**

<p align="center">
  <a href="assets/md/desorption/heterogeneous-350k/03-timestep-1152000-top.webp"><img src="assets/md/desorption/heterogeneous-350k/03-timestep-1152000-top.webp" width="30%" alt="Top view of the heterogeneous desorption end state at 1152000 timesteps"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/desorption/heterogeneous-350k/03-timestep-1152000-side.webp"><img src="assets/md/desorption/heterogeneous-350k/03-timestep-1152000-side.webp" width="30%" alt="Side view of the heterogeneous desorption end state at 1152000 timesteps"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/desorption/heterogeneous-350k/03-timestep-1152000-perspective.webp"><img src="assets/md/desorption/heterogeneous-350k/03-timestep-1152000-perspective.webp" width="22%" alt="Perspective view of the heterogeneous desorption end state at 1152000 timesteps"></a>
</p>

<p align="center"><sub>Water remains at the saved endpoint, together with a small persistent population at the Au interface; a longer run would be required to observe complete 350 K desorption.</sub></p>

The contrast with the clean sandwich endpoint is the central mechanistic result: the same molecular type can persist differently when initially adsorbed on Au rather than on water, so source-target material pairing matters in addition to temperature and molecular identity.

### Species-resolved desorption histories

<p align="center">
  <a href="assets/md/desorption/sandwich-350k/molecule-counts.png"><img src="assets/md/desorption/sandwich-350k/molecule-counts.png" width="47%" alt="Normalized species molecule counts versus timestep for the 350 kelvin sandwich desorption case"></a>&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="assets/md/desorption/heterogeneous-350k/molecule-counts.png"><img src="assets/md/desorption/heterogeneous-350k/molecule-counts.png" width="47%" alt="Normalized species molecule counts versus timestep for the 350 kelvin heterogeneous desorption case"></a>
</p>

<p align="center"><sub>Normalized molecule counts for the 350 K sandwich (left) and heterogeneous (right) trajectories.</sub></p>

## Historical Execution Environment and Reproducibility

The production workflow used a GPU-enabled Apptainer image named `lammps_patch_15Jun2023.sif`, one SLURM task, one GPU, 16 GB of memory, and 48-hour restart windows. After placing the archived image under `environment/`, the recorded launch pattern is:

```bash
apptainer exec --cleanenv --nv environment/lammps_patch_15Jun2023.sif \
  env LD_LIBRARY_PATH=/usr/local/fftw/lib:/usr/local/lammps/sm86/lib \
  /usr/local/lammps/sm86/bin/lmp -in input.lammps
```

Each complete case requires its LAMMPS input, initial data or restart state, `PARM.lammps`, molecule-definition files, force-field assets, and the recorded random seeds. Expected artifacts include the LAMMPS log, XYZ trajectory dumps, and selected restart/data snapshots.

For a tagged reproducible release, record the exact container digest or build recipe and reconcile the historical cluster-specific paths. See [the reproducibility notes](docs/REPRODUCIBILITY.md) for the case checklist and [the figure index](docs/FIGURE_INDEX.md) for source provenance.

## Repository Organization

```text
.
├── ctsp/                 # engineering-scale cases, parameter sets, and validation
├── lammps/
│   ├── deposition/       # sandwich, heterogeneous, and sensitivity cases
│   ├── desorption/       # temperature-ramp cases
│   ├── molecules/        # .mol definitions
│   └── force-fields/     # parameter and potential files
├── scripts/              # preprocessing and post-processing utilities
├── results/              # reference logs, plots, and selected trajectories
├── environment/          # container recipe/digest and runtime notes
├── papers/               # publications and manuscript
├── docs/                 # reproducibility and figure provenance
└── assets/               # README-ready visualizations
```

## Publications

- **Published conference paper:** Jacob Meyer, Lubos Brieda, and Joseph Wang, “Molecular dynamics simulations of heterogeneous molecular contaminant films,” *Proceedings of SPIE* 13628, 136280A (2025). [DOI: 10.1117/12.3066473](https://doi.org/10.1117/12.3066473) · [repository copy](papers/meyer-brieda-wang-2025-md-contaminant-films.pdf)
- **Follow-on manuscript:** Jacob Meyer, Lubos Brieda, and Joseph Wang, “Improved Accuracy in Molecular Transport Simulations Utilizing Multispecies Decomposition with QCM-Derived Sticking Coefficients.” [manuscript PDF](papers/meyer-brieda-wang-multispecies-qcm-transport-manuscript.pdf)
- **Experimental foundation:** E. Helou et al., “Experimental investigation of QCM-derived sticking coefficients for use in molecular transport simulations,” *Proceedings of SPIE* 12224 (2022). [DOI: 10.1117/12.2632195](https://doi.org/10.1117/12.2632195)

## Scope and Limitations

- The atomistic simulations are classical and non-reactive; bond-breaking chemistry is outside scope.
- Nanosecond-scale MD trajectories reveal mechanisms but do not directly reproduce mission-duration kinetics.
- Deposition cadence was constrained by computational cost; sensitivity cases quantify part of that trade.
- The 350 K heterogeneous run had not fully desorbed at its final saved frame.
- Atomistically derived CTSP closures remain a proposed next step, not a completed calibration.

## Credits and Reuse

The CTSP chamber figures and experimental image originate from Brieda et al. (2022) and are included as attributed system-modeling and validation context. The LAMMPS renders and Gaussian-model results originate from the project publications. Article PDFs and third-party figures remain subject to their respective publisher and author terms; the repository license should identify which code and original data are covered.
