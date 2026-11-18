# Persistence-Guided Validation of Torsional Pathways in 1,3-Butadiene

Reproducibility materials for the study by Dairo José Hernández Páez and Carlos Alberto Cadavid.

The earlier study, [Hernández et al., 2026](https://doi.org/10.3390/mca31030108), reported the torsional landscape and its sublevel-set persistent homology. The present package documents the scan used for the follow-up and the validation of stationary-point types and connectivity with unconstrained optimization, harmonic frequencies, and bidirectional intrinsic reaction coordinates (IRC).

## Reproduce without Gaussian

Run with Python 3.10 or later; no third-party Python packages are needed.

    python reproduce.py --output results

The script removes the duplicate periodic endpoint, builds the cycle graph and recomputes the persistence intervals against the reference CSV. It parses the Gaussian frequency jobs to recover the stationary-point energies, RRHO thermochemistry at 298.15 K and 1 atm, and directional barriers. It also checks normal termination, accepted point counts, minimum detection and positive frequencies of the reoptimized IRC endpoints, and assigns their basins using the signed C1–C4–C6–C8 dihedral. The endpoint check in this script uses torsion and frequencies; it does not recompute the supplementary RMSD column.

The expected result is two finite H0 intervals, one essential H0 interval, and one essential H1 interval on 720 unique angular samples. The six IRC branches connect s-cis to both gauche minima and each lateral saddle to s-trans and its corresponding gauche minimum.

## Files and recalculation

- `data/scan.txt`: 721 original angle–energy records (degrees, hartree), including the repeated periodic endpoint.
- `gaussian/`: input and gzip-compressed output (local filesystem paths redacted) for the scan, six stationary-point optimizations, six IRC branches, and six endpoint optimizations.
- `reference/persistence.csv`: reference persistence intervals in kcal/mol.
- `reproduce.py`: portable analysis and checks.

For new electronic structure calculations, use Gaussian 16 with an appropriate license. From a separate working directory, run the scan and stationary-point inputs, followed by the IRC inputs and then the endpoint optimizations. Keep each generated checkpoint in the working directory: subsequent inputs use `%oldchk` to refer to it. Checkpoints and Gaussian executables are not distributed here. The reference logs allow the checks above without rerunning any quantum chemistry calculations. This package does not reproduce the separately reported 520 K literature comparison or the geometric RMSD analysis.

Gaussian log paths identifying local directories have been redacted; the scientific output is unchanged.
