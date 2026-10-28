EEG Motor-Imagery Analysis with MNE-Python

Exploratory analysis of EEG recorded during imagined left- vs right-fist movement. The notebook covers loading and filtering the signal, epoching around cue events, estimating mu- and beta-band power over the motor cortex (C3, Cz, C4), and comparing the two conditions statistically.

Status: exploratory, single-subject analysis. Results are preliminary and are not evidence of an effect (see Limitations).

Dataset
PhysioNet EEG Motor Movement/Imagery Dataset (BCI2000 recordings), loaded directly through mne.datasets.eegbci.
Subject 1, run 4 (imagined left vs right fist).
64 EEG channels, sampled at 160 Hz, 125 s recording.
Events: T0 = rest (15), T1 = imagined left fist (8), T2 = imagined right fist (7).

No data files are stored in this repository. The notebook downloads the recording automatically on the first run.

Pipeline
Load the EDF recording with MNE, standardise channel names and set the electrode montage.
Filter with a 1–40 Hz zero-phase FIR band-pass filter.
Inspect the raw and filtered signal in the time domain and as power spectral density (Welch).
Epoch around T1 and T2 cues (−0.5 to 4 s) with baseline correction (−0.5 to 0 s).
Estimate band power with Welch's method at C3, Cz and C4:
mu: 8–13 Hz
beta: 13–30 Hz
Compare T1 vs T2 with Welch's independent-samples t-tests.
Results

Mean band power (µV²) per condition:

Channel	Mu, T1 (left)	Mu, T2 (right)	Beta, T1 (left)	Beta, T2 (right)
C3	162.4	126.8	193.1	182.7
Cz	160.0	144.1	185.5	167.7
C4	111.8	136.1	139.8	141.5

Welch's t-test p-values (T1 vs T2):

Channel	Mu p-value	Beta p-value
C3	0.0707	0.6499
Cz	0.2874	0.2950
C4	0.2753	0.9050

Mu and beta band power

PSD before filtering PSD after 1–40 Hz filtering

Interpretation. Mu-band power at C3 was about 22% lower during right-fist imagery than during left-fist imagery, and mu power at C4 was about 18% lower during left-fist imagery than during right-fist imagery. This is the direction expected from contralateral motor-cortex activity. However, no comparison reached p < 0.05 (smallest: C3 mu, p = 0.071), and none would survive a Bonferroni correction for six tests. Beta power showed no consistent lateralised pattern. These are exploratory trends in a very small sample, not a finding.

Limitations
One run from one participant (15 trials), so statistical power is low.
Epochs from the same recording are not fully independent, which may overstate the evidence in trial-level tests.
The comparison is of absolute band power between conditions over the whole epoch, not event-related desynchronisation (ERD) relative to a pre-cue baseline.
No artefact rejection (e.g., ICA) and no multiple-comparison correction in the tests.
Next steps
Extend to more subjects and to runs 4, 8 and 12.
Quantify ERD as percentage power change relative to baseline, using the imagery window only.
Test lateralisation across subjects with a paired test.
Add a time-frequency (Morlet) analysis and ICA for eye-blink artefacts.
Try a simple decoder (CSP + logistic regression) with cross-validation.
How to run
bash
git clone https://github.com/tejasreesivakumar22-neuro/EEG-Signal-Analysis-MNE.git
cd EEG-Signal-Analysis-MNE
pip install -r requirements.txt
jupyter notebook EEG_Motor_Imagery_Mu_Beta_Analysis.ipynb

An internet connection is needed on the first run to download the recording (about 1 MB). Run all cells from top to bottom; figures are saved to figures/.

Repository structure
EEG-Signal-Analysis-MNE/
├── EEG_Motor_Imagery_Mu_Beta_Analysis.ipynb
├── figures/
│   ├── psd_original.png
│   ├── psd_filtered.png
│   └── mu_beta_power.png
├── requirements.txt
└── README.md
Tools

Python, MNE-Python (1.13), NumPy, pandas, SciPy, Matplotlib.

Data references
Schalk, G., McFarland, D.J., Hinterberger, T., Birbaumer, N., Wolpaw, J.R. (2004). BCI2000: a general-purpose brain-computer interface (BCI) system. IEEE Transactions on Biomedical Engineering, 51(6), 1034–1043.
Goldberger, A.L., et al. (2000). PhysioBank, PhysioToolkit, and PhysioNet. Circulation, 101(23), e215–e220.
Author

Tejasree S, M.Sc. Biotechnology. Interested in cognitive neuroscience and BCI research.

