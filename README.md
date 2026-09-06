# Spacecraft Contamination Modeling & Simulation

**Multiscale research connecting QCM-informed, spacecraft-level molecular transport with atomistic molecular dynamics of heterogeneous contaminant films.**

<p align="center">
  <img src="assets/hero-ctsp-usc-molecular-transport.png" width="100%" alt="CTSP simulation of the USC vacuum-chamber geometry with predicted contaminant deposition and molecular number density fields">
</p>

<p align="center"><strong>Engineering scale:</strong> CTSP USC model. Surface shading shows deposited-film thickness; blue → green → yellow → red/pink indicates increasing molecular number density (green is intermediate, not a species). Source: Brieda et al. (2022).</p>

<p align="center">
  <img src="assets/hero-lammps-heterogeneous-film-side.png" width="62%" alt="Side view of the final heterogeneous contaminant film on gold">
  <img src="assets/hero-lammps-heterogeneous-film-perspective.png" width="35%" alt="Perspective view of the final heterogeneous contaminant film on gold">
</p>

<p align="center"><strong>Atomistic scale:</strong> side and perspective views of the final heterogeneous film, showing interfacial structure, roughness, clustering, and three-dimensional morphology. Source: Meyer, Brieda, and Wang (2025).</p>

## Overview

This repository documents nearly two years of multiscale spacecraft-contamination modeling and simulation research. It spans engineering-scale, QCM-derived multispecies gray-body molecular transport and atomistic LAMMPS simulations of contaminant-film deposition, morphological evolution, re-adsorption, and thermally activated desorption. It includes code, simulation inputs, results, a published conference paper, and a follow-on journal manuscript.

At the engineering scale, Quartz Crystal Microbalance thermogravimetric-analysis (QTGA) signals were separated through multi-Gaussian peak deconvolution into virtual contaminant species with temperature-dependent sticking coefficients and relative outgassing mass fractions. These parameters were propagated through CTSP particle-tracing and view-factor simulations to predict multiple-bounce, non-line-of-sight molecular transport. Across the reported validation cases, errors fell from hundreds or thousands of percent under reduced single-species or Arrhenius-based parameterizations to as low as **4.3%** for the four-Gaussian warm-wall case and **5.5%** for the six-Gaussian hot-to-cold case.

At the atomistic scale, classical non-reactive molecular dynamics resolved adsorbate–adsorbate and adsorbate–substrate interactions within heterogeneous water, hydrocarbon, and nitrogen films on gold. The simulations captured film restructuring, molecular clustering, re-adsorption, and material-pair-dependent desorption behavior omitted from conventional reduced-order models.

The long-term objective is **atomistically informed model reduction**: translating these interfacial-physics insights into species- and material-pair-dependent surface-interaction closures for higher-fidelity three-dimensional spacecraft-contamination transport simulations.

Put more simply, the atomistic simulations reveal how individual molecules stick, rearrange, and desorb, with the ultimate goal of using those insights to improve macroscale reduced-order models such as Dr. Lubos Brieda's Contamination Transport Simulation Program (CTSP).

## Project at a Glance

| Scale | Core question | Approach | Primary output |
|---|---|---|---|
| Engineering / system | Where does outgassed material travel and redeposit? | QTGA decomposition, multispecies parameterization, CTSP particle tracing, and view-factor transport | Deposition fields and experimentally correlated transport predictions |
| Atomistic / interface | How do mixed contaminant films stick, restructure, and desorb? | Classical non-reactive molecular dynamics in LAMMPS | Molecular trajectories, film morphologies, and species/material-dependent behavior |
| Cross-scale | Which microscopic effects should improve reduced-order transport models? | Physics interpretation and closure development | Future surface-interaction models for CTSP-class simulations |

## Technical Contributions

- Developed and evaluated a QCM-derived multispecies representation of spacecraft outgassing and deposition.
- Propagated temperature-dependent sticking behavior and relative species mass fractions through three-dimensional CTSP simulations.
- Built and executed LAMMPS cases for sequential and heterogeneous deposition, film restructuring, re-adsorption, and thermal desorption.
- Performed sensitivity studies, experimental correlation, and cross-model comparisons.
- Organized simulation inputs, analysis artifacts, results, and publications for reproducibility and continuation by future researchers.

## Multiscale Modeling Architecture

```mermaid
flowchart TD
    A["QTGA measurements"] --> B["Multi-Gaussian virtual species"]
    B --> C["CTSP particle and view-factor transport"]
    C --> D["System-level deposition predictions"]
    E["LAMMPS interfacial simulations"] --> F["Species and material-pair physics"]
    F -. "future reduced-order closures" .-> C
```

## QCM-Derived Multispecies Modeling

The measured QTGA response was decomposed into Gaussian components representing virtual contaminant species. Each component supplied a relative mass fraction and temperature-dependent sticking behavior for the engineering-scale transport model.

<p align="center">
  <img src="assets/qcm-multigaussian-peak-deconvolution.png" width="82%" alt="Two-, three-, and four-Gaussian decompositions of experimental QCM thermogravimetric-analysis data">
</p>

<p align="center"><em>Two-, three-, and four-Gaussian decompositions of an experimental QCM thermogravimetric-analysis signal.</em></p>

The hot-to-cold validation case demonstrates the resulting improvement in experimental correlation:

<p align="center">
  <img src="assets/hot-to-cold-validation-results.png" width="82%" alt="Hot-to-cold validation results showing 5.5 percent error for the six-Gaussian sticking-coefficient model">
</p>

<p align="center"><em>The six-Gaussian sticking-coefficient model reached 5.5% error, compared with 620–2502% for the evaluated activation-energy fits.</em></p>

## Engineering-Scale Molecular Transport

CTSP propagates contaminant species through three-dimensional geometry using particle tracing and view factors. This captures repeated surface interactions and non-line-of-sight transport pathways that cannot be represented by direct flux alone.

<p align="center">
  <img src="assets/ctsp-blue-origin-deposition-field.png" width="90%" alt="CTSP simulation of the Blue Origin vacuum-chamber configuration showing molecular number density and deposited-film thickness">
</p>

<p align="center"><em>Numerical deposition and molecular-density fields for the Blue Origin vacuum-chamber configuration. Source: Brieda et al. (2022).</em></p>

<p align="center">
  <img src="assets/blue-origin-vacuum-chamber-experiment.png" width="72%" alt="Blue Origin vacuum-chamber experiment with heated outgassing sample and QCM instrumentation">
</p>

<p align="center"><em>Physical vacuum-chamber configuration used to generate the experimental data underlying the system-scale model. Source: Brieda et al. (2022).</em></p>

## Atomistic Molecular Dynamics

The LAMMPS simulations modeled heterogeneous water, hydrocarbon, and nitrogen contamination on a gold substrate. Sequential “sandwich” cases and simultaneously deposited heterogeneous cases isolated the influence of deposition history, temperature, molecular interactions, and initial morphology.

### Sequential “Sandwich” Deposition

<p align="center">
  <img src="assets/lammps-sandwich-water-layer-side.png" width="62%" alt="Side view of the sandwich case after formation of the first water layer">
  <img src="assets/lammps-sandwich-water-layer-perspective.png" width="35%" alt="Perspective view of the sandwich case after formation of the first water layer">
</p>

<p align="center"><em>Stage 1: the first water layer forms on the gold substrate.</em></p>

<p align="center">
  <img src="assets/lammps-sandwich-hydrocarbon-layer-side.png" width="62%" alt="Side view of the sandwich case after formation of the hydrocarbon layer">
  <img src="assets/lammps-sandwich-hydrocarbon-layer-perspective.png" width="35%" alt="Perspective view of the sandwich case after formation of the hydrocarbon layer">
</p>

<p align="center"><em>Stage 2: a hydrocarbon layer forms over the initial water film.</em></p>

<p align="center">
  <img src="assets/lammps-sandwich-final-layered-film-side.png" width="62%" alt="Side view of the final sandwich deposition configuration after the third water layer formed">
  <img src="assets/lammps-sandwich-final-layered-film-perspective.png" width="35%" alt="Perspective view of the final sandwich deposition configuration after the third water layer formed">
</p>

<p align="center"><em>Stage 3: the final water layer completes the sequentially deposited film.</em></p>

### Heterogeneous Deposition and Film Growth

<p align="center">
  <img src="assets/lammps-heterogeneous-first-monolayer-side.png" width="62%" alt="Side view of a heterogeneous contaminant film at timestep two million as its first monolayer approaches completion">
  <img src="assets/lammps-heterogeneous-first-monolayer-perspective.png" width="35%" alt="Perspective view of a heterogeneous contaminant film at timestep two million as its first monolayer approaches completion">
</p>

<p align="center"><em>At timestep 2 million, the first heterogeneous monolayer is nearly complete.</em></p>

The completed heterogeneous film is shown in the side and perspective hero views above; together they expose both the interface-normal structure and the three-dimensional morphology without repeating all three camera angles throughout the README.

### Thermally Activated Restructuring and Desorption

<p align="center">
  <img src="assets/lammps-sandwich-thermal-desorption.png" width="100%" alt="LAMMPS sandwich contaminant film during 450 kelvin restructuring and desorption">
</p>

<p align="center"><em>At 450 K, the initially layered film restructures; the hydrocarbon layer desorbs before the remaining water film.</em></p>

### Sensitivity and Model Definition

<p align="center">
  <img src="assets/lammps-deposition-interval-sensitivity.png" width="100%" alt="Water-only LAMMPS deposition-interval sensitivity cases">
</p>

<p align="center"><em>Water-only deposition-interval sensitivity study used to assess dependence on numerical deposition cadence.</em></p>

The atomistic models used established representations including TIP4P/Ice, OPLS-AA, CHARMM36, Lennard-Jones interactions, and an explicit gold substrate.

<p align="center">
  <img src="assets/molecular-models-and-force-fields.png" width="100%" alt="Molecular species, visualizations, force fields, and nonbonded parameters used in the LAMMPS simulations">
</p>

<p align="center"><em>Molecular species and force-field definitions used in the LAMMPS simulations.</em></p>

## Key Results

- Multi-Gaussian virtual-species models markedly improved agreement with QCM measurements over the evaluated reduced single-species and activation-energy approaches.
- The best reported warm-wall and hot-to-cold cases reached **4.3%** and **5.5%** error, respectively.
- Atomistic cases revealed deposition-history-dependent morphology, clustering, dynamic restructuring, re-adsorption, and species-dependent desorption.
- The results identify physical behavior that can inform richer sticking, residence-time, and desorption closures in future system-scale transport tools.

## Repository Guide

<!-- Replace these example paths with the final repository directory names. -->

| Directory | Contents |
|---|---|
| `ctsp/` | Engineering-scale model inputs, parameter sets, and analysis |
| `lammps/` | Molecular definitions, force-field parameters, data files, and LAMMPS input scripts |
| `scripts/` | Python preprocessing, post-processing, and validation utilities |
| `results/` | Selected plots, tables, and visualization outputs |
| `papers/` | Conference paper, follow-on manuscript, and supporting reference material |
| `environment/` | Container information and software/runtime notes |

## Reproducing the Simulations

<!-- Replace this outline with exact, verified commands after the final files are organized. -->

1. Build or pull the documented LAMMPS runtime environment.
2. Place the required molecular definitions and force-field parameter files beside the selected input case.
3. Run one documented reference case using its batch script or direct LAMMPS command.
4. Post-process trajectory and thermodynamic output using the accompanying Python scripts.
5. Compare the generated result against the reference output in `results/`.

## Publications

- **Published conference paper:** molecular-dynamics simulations of heterogeneous spacecraft contaminant films — Meyer, Brieda, and Wang (2025).
- **Follow-on journal manuscript:** QCM-derived multi-Gaussian, multispecies spacecraft-contamination transport modeling.
- **Experimental foundation:** Brieda et al. (2022), *Experimental Investigation of QCM-Derived Sticking Coefficients for Use in Molecular Transport Simulations*.

## Limitations and Future Work

The molecular-dynamics cases are classical and non-reactive, and their finite length and scale do not directly reproduce mission timescales. The next research step is to reduce atomistic observations into calibrated species- and material-pair-dependent sticking, residence-time, or desorption closures, then evaluate those closures in CTSP-class simulations against experimental data.

## Figure Credits

The USC and Blue Origin CTSP images and Blue Origin chamber photograph originate from Brieda et al. (2022) and are included as the experimental and system-modeling context used by this research. The LAMMPS figures and Gaussian-model results originate from the project's conference paper and follow-on manuscript. Before public release, replace screenshots with author-controlled source exports where available and confirm applicable publisher reuse requirements.
