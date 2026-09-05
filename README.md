# Spacecraft Contamination Modeling & Simulation

This repository documents a multiscale spacecraft-contamination modeling and simulation effort spanning macro-scale QCM-derived multispecies gray-body molecular transport simulations, and atomistic-level LAMMPS simulations of heterogeneous contaminant-film deposition, morphological evolution, re-adsorption, and thermally activated desorption. It includes code, simulation inputs, results, a published conference paper, and a follow-on journal manuscript developed all over the course of nearly two years of work. 

With the macro-scale model, Quartz-Crystal Microbalance thermogravimetric-analysis (QTGA) signals are separated through multi-Gaussian peak deconvolution into virtual contaminant species with temperature-dependent sticking coefficients and relative outgassing mass fractions. These parameters were propagated through CTSP particle-tracing and view-factor simulations to predict multiple-bounce, non-line-of-sight molecular transport. Across the reported cases, the multispecies parameterization improved experimental correlation by up to nearly three orders of magnitude relative to the legacy single-species (unitary) "sticking-coefficient" model, reaching 5.5% error (compared with experiment) in the six-Gaussian hot-to-cold validation case.

At the atomistic scale, classical non-reactive molecular dynamics simulations were run to resolve adsorbate–adsorbate and adsorbate–substrate interactions within heterogeneous water, hydrocarbon, and nitrogen films on gold. The simulations capture dynamic film restructuring, molecular clustering, re-adsorption, and material-pair-dependent desorption behavior omitted from conventional reduced-order models.

The long-term objective is atomistically informed model reduction: translating these interfacial-physics insights into species- and material-pair-dependent surface-interaction closures for higher-fidelity three-dimensional macro-scale spacecraft-contamination transport simulations in the future. 

Put more simply, the atomistic simulations reveal the underlying behavior (e.g. how individual molecules stick, rearrange, and desorb, etc.), with the ultimate goal being to using those insights to improve the accuracy of macro-scale reduced-order spacecraft contamination models such as Dr. Lubos Brieda's Contamination Transport Simulation Program (CTSP) codes. 

