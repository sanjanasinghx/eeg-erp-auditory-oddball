# Auditory Oddball ERP Pipeline (P300, N200, MMN)

ERP analysis of one subject's auditory oddball EEG in MNE-Python: preprocessing, ICA artifact removal, epoching, P300/N200 stats, the mismatch negativity, and a comparison of reference electrodes.

Coursework for CG 614 Human Brain Mapping, IIT Gandhinagar, 2026. [Report (PDF)](./report.pdf) | [Academia.edu](https://telaviv.academia.edu/SanjanaSingh)

## Data

64-channel BioSemi auditory oddball EEG from OpenNeuro [ds003061](https://openneuro.org/datasets/ds003061) (*P300 EEG auditory oddball*), sub-001. BIDS format, loaded from the `.set` file with `mne.io.read_raw_eeglab()`.

## Pipeline

1. Cleaning: bad-channel detection (log-variance z-score), spherical-spline interpolation, FIR bandpass (0.1-30 Hz), 50 Hz notch, ICA (FastICA, 15 components) dropping components correlated with Fp1/Fp2 (ocular), average reference.
2. Epoching: -200 to +800 ms around stimulus onset, 200 ms pre-stimulus baseline, 100 µV peak-to-peak rejection.
3. Components: P300 (Pz, 300-500 ms) and N200 (Fz, 180-280 ms), normality check, one-tailed group comparisons, topomaps at peak latency.
4. MMN: oddball minus standard difference wave at Fz.
5. Reference comparison: average vs. Cz vs. linked mastoids (TP7+TP8) vs. Fpz, to see how the reference changes the P300 topography.

## Results

| Component | Channel | Window | Result |
| --- | --- | --- | --- |
| P300 | Pz | 300-500 ms | Mann-Whitney *U* = 23,979, *p* < .001, *r* = .32; peak 6.69 µV at 406 ms |
| N200 | Fz | 180-280 ms | *t*(509) = 5.00, *p* < .001, Cohen's *d* = 0.60; peak -3.96 µV at 254 ms |
| MMN | Fz | 150-250 ms | -4.92 µV at ~250 ms (difference wave) |

The average reference gave the cleanest centro-parietal P300 topography. With a Cz reference the P300 was suppressed, since Cz itself is active during the response.

Libraries: `mne`, `numpy`, `scipy.stats`, `matplotlib`.

## Running it

```bash
git clone https://github.com/sanjanasinghx/eeg-erp-auditory-oddball.git
cd eeg-erp-auditory-oddball
pip install -r requirements.txt
# download sub-001 from OpenNeuro ds003061 into ./data/
jupyter notebook eeg_erp_auditory_oddball.ipynb
```

## Related repos

- [`infant-eeg-spectral-fooof`](https://github.com/sanjanasinghx/infant-eeg-spectral-fooof): infant EEG spectral and aperiodic analysis
- [`rsfmri-connectivity-cpm`](https://github.com/sanjanasinghx/rsfmri-connectivity-cpm): resting-state fMRI connectivity and CPM

## References

- Polich, J. (2007). Updating P300: An integrative theory of P3a and P3b. *Clinical Neurophysiology*, 118(10).
- Näätänen, R., Paavilainen, P., Rinne, T., & Alho, K. (2007). The mismatch negativity (MMN). *Clinical Neurophysiology*, 118(12).
- Dien, J. (1998). Issues in the application of the average reference. *Behavior Research Methods*.

## License

Code: MIT. Report text: CC BY 4.0.
