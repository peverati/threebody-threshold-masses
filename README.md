# threebody-threshold-masses

Code and data for

> R. Peverati, *Where does a molecule begin? Threshold masses of three-particle Coulomb systems and a predictive theory of their Born–Oppenheimer offsets* (submitted to *Phys. Chem. Chem. Phys.*).

The repository contains the three in-house solvers used in the paper and, for every table and figure, the scripts and the result files they were made from. Everything is plain Python; no compiled code, no random numbers.

Archived on Zenodo: [10.5281/zenodo.24398482](https://doi.org/10.5281/zenodo.24398482) (release v1.0.0). Licences: code MIT (`LICENSE`), data CC BY 4.0 (`LICENSE-DATA`).

## Contents

| folder | what it is |
|---|---|
| `solvers/threebody/` | exact variational solver for three Coulomb particles {X⁺, X⁺, e⁻} (and unequal masses) in a perimetric Laguerre basis; any total angular momentum J (bipolar harmonics), excited roots, observables, pair distributions, double-double matrix assembly for near-threshold states |
| `solvers/bo_h2plus/` | Born–Oppenheimer baseline: clamped-nuclei 1sσg curve to ~1e-12, diagonal adiabatic correction, all vibration–rotation levels for any mass, BO and BO+ad threshold masses |
| `solvers/harmonic_model/` | exactly solvable harmonic three-body models used to calibrate the descriptors (fidelity susceptibilities, ESI Section S3.12) |
| `paper_data/` | one folder per calculation: run scripts, analysis scripts, result files (`.csv`, `.json`, `.txt`, `.npz`) and a README or RESULT.md describing each |
| `paper_data/figures_final/` | `make_figures.py`, `make_tables.py` and `summary_values.py` regenerate every figure and table of the paper from the result files only |

Each solver folder has its own README with the method, the validation and the tests.

## Where each figure and table comes from

| paper | file in `paper_data/figures_final/` | main data folders |
|---|---|---|
| Fig. 1 (mass functions) | `F8_massfunc` | `bo_offset/massfunc/` |
| Fig. 2 (BO offsets, blind tests) | `F3_offsets` | `bo_offset/`, `critical_masses*/`, `thresholds_Lv/`, `threshold_map/`, `unequal_masses/` |
| Fig. 3 (bound levels and threshold map) | `F1_levels` | `critical_masses*/`, `rotational/`, `thresholds_Lv/`, `threshold_map/`, `threshold_map_L7hp/` |
| Fig. 4 (threshold laws) | `F2_threshold` | `critical_masses/`, `critical_masses_v4/`, `thresholds_Lv/` |
| Fig. 5 (size near threshold) | `F2h_halo` | `critical_masses/`, `critical_masses_v4/` |
| Fig. 6 (muonic ions) | `F7_muonic` | the thresholds of Table 1 (`summary_values.py`) |
| Fig. 7 (ground-state crossover) | `F4_crossover` | `mass_scans/` (markers from `crossover_width/`, `king2013/`, `fidelity/`, `entanglement/`) |
| Fig. 8 (approach to the BO limit) | `F5_BO_approach` | `mass_scans/` |
| Fig. 9 (electron–nuclear entanglement) | `F6_entanglement` | `entanglement/` |
| Tables 1–4 | `tables.md`, `tables.tex` (`make_tables.py`) | as listed in `paper_data/figures_final/README.md` |

Further folders support the ESI: `king2013/` (comparison with King, Longford and Cox), `codata2012/` and `codata2022/` (mass constants), `fidelity_models/` and `mullerherold/` (solvable models), `rotational/` (J > 0 validation).

**Pre-specified predictions.** The parameter-free predictions of the threshold offsets were frozen before the exact calculations; the frozen files (`PREDICTION_*.md`, `prediction_*.json`) and their SHA-256 hashes are in `critical_masses_v4/`, `thresholds_Lv/`, `threshold_map/` and `unequal_masses/`. The frozen files are byte-identical to the versions hashed at the time; `FREEZE_NOTES.md` in each folder records what had and had not been computed before each freeze.

## Requirements and installation

Python ≥ 3.10 with NumPy, SciPy, SymPy, mpmath, pandas, matplotlib and threadpoolctl; pytest for the tests. The versions used for the paper are pinned in `requirements.txt`:

    python3 -m pip install -r requirements.txt

Nothing needs to be installed from this repository: the scripts find the solvers through relative paths, so run them from the folders indicated in each README (most run from the repository root).

BLAS threads: the solvers limit themselves by default (4 threads for `threebody` and `bo_h2plus`, 2 for `harmonic_model`); set `THREEBODY_BLAS_THREADS`, `BO_H2PLUS_THREADS` or `HARMONIC_BLAS_THREADS` to change this.

## Quick checks

    cd solvers/threebody      && python3 -m pytest -q tests
    cd solvers                && python3 -m pytest -q bo_h2plus/tests
    cd solvers/harmonic_model && python3 -m pytest -q tests
    cd paper_data/figures_final   && python3 summary_values.py && python3 make_figures.py && python3 make_tables.py

Each line starts from the repository root. Run times on an Apple-silicon laptop: `harmonic_model` (49 tests) about 5 s, `bo_h2plus` (44) about 15 s (it builds and caches its tables on the first run), `threebody` (38) about 25 min.

`summary_values.py` compares every number quoted in the figures and tables with the result files.

## Notes

* Large production runs (threshold maps) took hours to days on 8–20 cores; their outputs are included, so no calculation needs to be repeated to check the paper.

## Licence

Code: MIT (`LICENSE`). Data and result files: CC BY 4.0 (`LICENSE-DATA`). If you use them, please cite the paper and this repository (`CITATION.cff`).
