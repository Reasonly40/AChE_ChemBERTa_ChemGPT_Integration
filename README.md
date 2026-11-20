# AChE de novo design with ChemBERTa and ChemGPT

Code accompanying *Generating Acetylcholinesterase Potential Inhibitors through Foundational Models Integration*
(Roeroe, Sengkey, Tallei, Masengi). One notebook covers the full pipeline:

1. ChEMBL data curation (CHEMBL220, IC50 ≤ 1000 nM)
2. ChemBERTa → untrained linear projection → ChemGPT generation (plus optional ablation baselines)
3. SELFIES→SMILES conversion, novelty, Lipinski (≤ 1 violation), Klekota–Roth Tanimoto filtering, generative metrics, attention map
4. GNINA redocking and candidate docking (3 receptors × 3 seeds)
5. GROMACS system set-up, production and trajectory analysis

`notebooks/AChE_pipeline_ChemBERTa_ChemGPT.ipynb`

## What is and is not included
- Included: all code needed to regenerate the library, filter it, dock it and analyse MD trajectories.
- Not included: the generated library, docking poses and trajectories. They are in the article's Supplementary Files S1–S4
  [and in the archive at DOI: *add after the Zenodo release*].
- Done outside the notebook: ACPYPE web-server ligand parameterisation, SwissADME, ProTox 3.0, Discovery Studio diagrams.
- `KRFingerprints.py` and `krfp.json` (Klekota–Roth keys) must be supplied in `ache_workdir/krfp/`; check the licence of the implementation you use.

## Run
```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook notebooks/AChE_pipeline_ChemBERTa_ChemGPT.ipynb
```
GNINA 1.3.2, Open Babel 3.1.1 and GROMACS must be installed separately (see `requirements.txt`). A CUDA GPU is needed for practical generation, docking and MD.
Cells marked `# EXPECTED:` print the value reported in the article so each part can be checked.

## Ligand numbering
"Ligand 447 / 712 / 1130" is the index (0–3498) of the reference molecule whose embedding conditioned generation, not a row number among the 79 candidates.

## Reproducibility notes
- Generation uses one global seed (42); resuming an interrupted run changes the random stream.
- Docking seeds are 0, 1, 2. MD velocities are generated randomly (`gen_seed = -1`).
- GROMACS versions differ by stage (2021.4 preparation/equilibration, 2026.1 and 2025.4 production, 2026.1 analysis); record `gmx --version` for your runs.

## Citation
See `CITATION.cff`. License: MIT.
