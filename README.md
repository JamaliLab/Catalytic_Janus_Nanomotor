
<p align="center">
  <img src="Janus.jpg" alt="Catalytic Janus nanomotors" width="100%">
</p>

# Polarity-resolved imaging reveals active transport in catalytic Janus nanomotors

This repository contains the Jupyter notebook used to reproduce the trajectory, transport, and polarity-resolved analyses associated with the ChemRxiv manuscript:

**“Polarity-resolved imaging reveals active transport in catalytic Janus nanomotors.”**

Analysis notebook:

`Catalytic_Janus_Nanomotor.ipynb`

The corresponding dataset is publicly available from the Jamali Lab on Hugging Face:

https://huggingface.co/datasets/JamaliLab/Catalytic_Janus_Nanomotor/tree/main

## Overview

The notebook contains the analysis used to generate the quantitative plots for the manuscript, including:

- representative Janus nanomotor trajectories;
- ensemble-averaged time-averaged mean-squared displacement (MSD) analysis;
- transport statistics for active (`H2O2`) and control (`PBS`) conditions;
- displacement and orientation analysis;
- polarity-resolved comparison of the Pt-cap orientation and direction of motion;
- angular-difference and `cos(theta_Pt - theta_ld)` distributions;
- statistical tests of directional bias.

The notebook is organized into three main sections:

1. **Notebook Initialization**
2. **Transport Statistics**
3. **Polarity Analysis**

## Data

Download the complete dataset from:

**JamaliLab/Catalytic_Janus_Nanomotor**

https://huggingface.co/datasets/JamaliLab/Catalytic_Janus_Nanomotor/tree/main

The notebook uses two types of input data.

### 1. Trajectory CSV files

The transport-analysis section loads:

```text
H2O2.csv
PBS.csv
```

These files contain the particle trajectories used for the trajectory plots, MSD calculations, and transport analysis.

The notebook expects columns including:

```text
particle_id
frame
x
y
angle
```

### 2. Polarity-analysis folders

The polarity-resolved analysis uses separate condition folders containing individual particle folders. Each particle folder contains a trajectory file and a directory of segmentation masks.

The expected dataset organization is:

```text
Catalytic_Janus_Nanomotor/
├── H2O2.csv
├── PBS.csv
├── H2O2/
│   ├── 0/
│   │   ├── trajectories.csv
│   │   └── masks/
│   │       ├── 0000.png
│   │       ├── 0010.png
│   │       └── ...
│   ├── 1/
│   │   ├── trajectories.csv
│   │   └── masks/
│   └── ...
└── PBS/
    ├── 0/
    │   ├── trajectories.csv
    │   └── masks/
    ├── 1/
    │   ├── trajectories.csv
    │   └── masks/
    └── ...
```

The polarity analysis matches mask images to trajectory frames using the leading frame number in each mask filename.

## Downloading the dataset

### Option 1: Git

If Git LFS is installed:

```bash
git lfs install
git clone https://huggingface.co/datasets/JamaliLab/Catalytic_Janus_Nanomotor
```

### Option 2: Hugging Face CLI

Install the Hugging Face Hub command-line tool:

```bash
pip install -U huggingface_hub
```

Then download the dataset:

```bash
hf download JamaliLab/Catalytic_Janus_Nanomotor \
    --repo-type dataset \
    --local-dir Catalytic_Janus_Nanomotor
```

## Environment

The analysis is written in Python and uses standard scientific Python packages.

A minimal environment can be installed with:

```bash
pip install numpy pandas matplotlib scipy scikit-image pillow pingouin jupyter
```

Principal packages imported by the notebook include:

- NumPy
- pandas
- Matplotlib
- SciPy
- scikit-image
- Pillow
- Pingouin

Launch the notebook with:

```bash
jupyter notebook Catalytic_Janus_Nanomotor.ipynb
```

or:

```bash
jupyter lab
```

## Setting the data paths

The original notebook uses `...` to indicate the local directory in which the downloaded dataset is stored. Before running the notebook, replace `...` with the full path to the downloaded `Catalytic_Janus_Nanomotor` dataset.

### Trajectory CSV files

The original notebook contains:

```python
df_active = pd.read_csv(r'...\H2O2.csv')
df_non = pd.read_csv(r'...\PBS.csv')
```

For example, on Windows, if the dataset was downloaded to:

```text
C:\Users\username\Documents\Catalytic_Janus_Nanomotor
```

use:

```python
df_active = pd.read_csv(
    r'C:\Users\username\Documents\Catalytic_Janus_Nanomotor\H2O2.csv'
)

df_non = pd.read_csv(
    r'C:\Users\username\Documents\Catalytic_Janus_Nanomotor\PBS.csv'
)
```

On macOS or Linux, the corresponding paths could be:

```python
df_active = pd.read_csv(
    '/Users/username/Documents/Catalytic_Janus_Nanomotor/H2O2.csv'
)

df_non = pd.read_csv(
    '/Users/username/Documents/Catalytic_Janus_Nanomotor/PBS.csv'
)
```

### H2O2 polarity analysis

The original notebook contains:

```python
ROOT = r"...\H2O2"
```

Replace `...` with the location of the downloaded dataset.

For example, on Windows:

```python
ROOT = r"C:\Users\username\Documents\Catalytic_Janus_Nanomotor\H2O2"
```

On macOS or Linux:

```python
ROOT = "/Users/username/Documents/Catalytic_Janus_Nanomotor/H2O2"
```

### PBS polarity analysis

The original notebook contains:

```python
ROOT = r"...\PBS"
```

Replace `...` with the location of the downloaded dataset.

For example, on Windows:

```python
ROOT = r"C:\Users\username\Documents\Catalytic_Janus_Nanomotor\PBS"
```

On macOS or Linux:

```python
ROOT = "/Users/username/Documents/Catalytic_Janus_Nanomotor/PBS"
```

No other change to the folder structure is required if the complete Hugging Face dataset is downloaded.

## Running the analysis

1. Download the full dataset from Hugging Face.
2. Open `Catalytic_Janus_Nanomotor.ipynb`.
3. Replace `...` in the CSV paths with the local path to the downloaded dataset.
4. Replace `...` in the `ROOT` definition for the H2O2 polarity analysis.
5. Replace `...` in the `ROOT` definition for the PBS polarity analysis.
6. Run the notebook cells sequentially from top to bottom.

## Analysis parameters

Several analysis parameters are defined directly in the notebook. These include:

```text
FPS = 10
PX_NM = 2.0
SUBSAMPLE = 10
WINDOW_LD = 6
D_EFF = 235
LAG_TIME_S = 1
MIN_AREA = 1000
RANSAC_ITERS = 300
RANSAC_THRESH = 2.0
RANSAC_SEED = 0
```

These values correspond to the manuscript analysis and should be left unchanged when reproducing the reported results.

## Polarity-resolved analysis

For each particle, the notebook combines its tracked trajectory with the corresponding segmentation masks.

The Pt-cap orientation is estimated from the particle mask and compared with the local direction of motion. The directional relationship is summarized by the angular difference between the Pt orientation and local displacement direction and by:

```text
cos(theta_Pt - theta_ld)
```

With the convention used in the notebook:

- positive values correspond to preferential motion toward the Pt side;
- negative values correspond to preferential motion with the Pt side trailing;
- values near zero indicate no strong forward/trailing preference.

The notebook also reports circular-direction statistics and statistical tests used in the manuscript analysis.

## Reproducing manuscript figures

Run the notebook sequentially to generate the trajectory, MSD, transport-statistics, and polarity-resolved plots used in the manuscript.

Most plotting cells display figures directly using:

```python
plt.show()
```

The notebook also defines:

```python
figure_output = r'\PtPS Figures'
```

If figures are to be saved automatically, this path can be changed to a desired local output directory and `plt.savefig(...)` can be added where needed.

## Citation

If you use this dataset or analysis code, please cite the associated ChemRxiv manuscript:

> **Polarity-resolved imaging reveals active transport in catalytic Janus nanomotors.**

Please use the final bibliographic information and DOI provided on the ChemRxiv manuscript page.

## Data availability

The data required to reproduce the analyses in this notebook are publicly available from the Jamali Lab Hugging Face dataset repository:

https://huggingface.co/datasets/JamaliLab/Catalytic_Janus_Nanomotor/tree/main

## Contact

For questions regarding the dataset, analysis, or manuscript, please contact the authors through the Jamali Lab.
