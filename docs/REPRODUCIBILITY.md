# Reproducibility and Release Checklist

This research archive preserves representative inputs, restart states, scheduler scripts, results, and publications. It should not be described as turnkey until every case has been assembled with the dependencies and environment metadata listed below.

## Complete Case Bundle

Each runnable LAMMPS case should contain or reference:

- one canonical `.lammps` input;
- its initial `.data` or restart state;
- `PARM.lammps` and every included parameter/potential file;
- all referenced molecule definitions, including TIP4P/Ice water, methane, N₂, decane, and toluene;
- the random seeds used for molecule placement and velocity initialization;
- a case-local SLURM script with no user-specific absolute paths;
- one short reference log and a lightweight expected-result artifact;
- a README stating temperature schedule, timestep, run length, deposition interval, outputs, approximate hardware/runtime, and known caveats.

## Historical Execution Environment

The supplied production scheduler scripts record:

| Setting | Historical value |
|---|---|
| Scheduler | SLURM |
| Container runtime | Apptainer with NVIDIA GPU passthrough |
| Image filename | `lammps_patch_15Jun2023.sif` |
| LAMMPS executable | `/usr/local/lammps/sm86/bin/lmp` inside the image |
| Requested resources | 1 task, 1 CPU, 1 GPU, 16 GB RAM, 48 hours |
| OpenMP threads | 1 |

Portable launch pattern:

```bash
apptainer exec --cleanenv --nv environment/lammps_patch_15Jun2023.sif \
  env LD_LIBRARY_PATH=/usr/local/fftw/lib:/usr/local/lammps/sm86/lib \
  /usr/local/lammps/sm86/bin/lmp -in input.lammps
```

For public release, add the container recipe and immutable digest. If redistribution of the `.sif` is not permitted, document how to rebuild the custom image from the corresponding [NVIDIA LAMMPS container](https://catalog.ngc.nvidia.com/orgs/hpc/containers/lammps) and identify every local patch.

## Reference Outputs and Verification

For each case, retain:

1. the LAMMPS version banner and package configuration;
2. the first and last thermodynamic records;
3. atom and molecule counts before and after the run;
4. the final restart/data snapshot;
5. a small trajectory excerpt or selected state image;
6. an expected qualitative result and, where available, a numeric tolerance.

Recommended checks include energy/temperature stability during equilibration, molecule conservation before evaporation is enabled, monotonic removal counts during desorption, and comparison of final morphology or normalized species counts with the reference results.

## Source-Integrity Items to Resolve

- The archived LaTeX source refers to `WaterScaling_5k_Side.png`, while the file is named `WaterScaling_5k_side.png`.
- It refers to `HeteroDeposition_TS2MIL_Top.png`, while the file is named `HeteroDeposition_TS2Mil_Top.png`.
- The ~372k sandwich triplet uses `373` in the top/side filenames and `372` in the perspective filename.
- The historical 566k heterogeneous 450 K triplet uses `556` in the top filename and `566` in the side/perspective filenames.
- The bundled `main.tex` and compiled `output.pdf` are not the same source revision. Rebuild the publication bundle from a reconciled tagged release.
- The early 450 K trajectories used a known erroneous toluene-hydrogen Lennard-Jones parameterization. Keep them under a clearly labeled `legacy-diagnostic/` path if retained.
- Historical scripts contain user- and cluster-specific absolute paths; replace them with case-relative paths or environment variables.

## Public-Release Checks

- Add an explicit software/data license.
- Confirm permissions for publisher-formatted PDFs and third-party figures.
- Remove account names, email addresses, and cluster-specific paths that should not be public.
- Scan the repository for credentials, tokens, private URLs, and controlled/proprietary data.
- Tag the exact code/data state associated with each paper.
- Add checksums for large restart files and container artifacts.

