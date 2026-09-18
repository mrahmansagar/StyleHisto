# StyleHisto

**StyleHisto** is a tensorflow-based framework designed for style transfer, image-to-image translation of histology patch-level images. It supports both paired (Pix2Pix) and unpaired (CycleGAN) translation workflows, along with registration utilities to align histological sections.

---

## Features

- **Generative Style Transfromation**:
  - **Pix2Pix**: Supervised paired image-to-image translation.
  - **CycleGAN**: Unsupervised unpaired image-to-image translation.
- **Histology Image Registration**:
  - Elastic and affine registration pipelines for aligning stained tissue sections.
---

## Repository Structure

```text
StyleHisto/
├── notebooks/
│   ├── cycleGAN.ipynb          # Interactive notebook for CycleGAN experiments
│   └── pix2pix.ipynb           # Interactive notebook for Pix2Pix experiments
├── src/
│   ├── __init__.py
│   ├── dataloader.py           # Custom PyTorch Dataset and DataLoader implementations
│   ├── models.py               # Generator and Discriminator architectures for pix2pix and cycleGAN model. 
│   ├── registration.py         # Core registration algorithms and transformations
│   ├── reg_pipeline.py         # End-to-end registration pipeline execution
│   └── reg_utils.py            # Utility functions for warping, metrics, and IO
├── elastic_registation.py      # Standalone script for elastic registration
├── train_cycleGAN.py           # Training entry point for CycleGAN
├── train_pix2pix.py            # Training entry point for Pix2Pix
├── LICENSE                     # License file
└── README.md                   # Project documentation
└── requirments.txt             # Project dependency packages 
```

---

## Installation & Setup

### 1. Prerequisites

- Python 3.10+ recommended
- [CUDA Toolkit](https://developer.nvidia.com/cuda-toolkit) (if using GPU acceleration)

---

### 2. Clone the Repository

```bash
git clone https://github.com/mrahmansagar/StyleHisto.git
cd StyleHisto
```

---

### 3. Set Up a Virtual Environment

You can create an isolated virtual environment using either Python's built-in `venv` or `conda`.

```bash
# Create virtual environment named '.venv'
python3 -m venv .venv

# Activate the virtual environment
source .venv/bin/activate
```

#### Using Conda

```bash
# Create a conda environment with Python 3.9
conda create -n stylehisto python=3.9 -y

# Activate the environment
conda activate stylehisto
```

---

### 4. Install Dependencies

Upgrade `pip` and install the required dependencies:

```bash
python3 -m pip install --upgrade pip
```

#### Install tensorflow

Install tensorflow according to your platform and CUDA version (refer to [tensorflow.org](https://www.tensorflow.org/install/pip) for your exact setup):

Recommended tensorflow version >= 2.0,  =< 2.15

```bash
# example installation on Linux machine 
python3 -m pip install 'tensorflow[and-cuda]==2.15.0.post1'
```

#### Install Required Packages

```bash
pip install -r requirements.txt
```

---

## Dataset Preparation

Organize your histological patches or images under any directory.

### For Pix2Pix (Paired Data)
Pix2Pix expects aligned/paired images:
```text
pix2pix/train
├── ct/
│   ├── 001.tif
│   └── ...
└── histo/
    ├── 001.tif                     # corresponding histo pair of ct/001.tif 
    └── ...
```

### For CycleGAN (Unpaired Data)
CycleGAN expects two distinct domain folders:
```text
cyclegan/train
├── domainA/                        # i.e. CT images 
│   ├── 001.tif
│   └── ...
└── domainB/                        # i.e. histo images 
    ├── 001.tif 
    └── ...
```

---

## Usage

### Training Pix2Pix

Train paired image-to-image translation using `train_pix2pix.py`:

```bash
python3 train_pix2pix.py 
    --src_dir "./data/processed/colon/ct" 
    --tar_dir "./data/processed/colon/histo" 
    --color_mode "rgb" 
    --epochs 100 
    --summary_interval 10
    --output_dir ./training_output
```
Get the full list of arguments by 
```bash
python3 train_pix2pix.py --help
```

### Training CycleGAN

Train unpaired translation using `train_cycleGAN.py`:

```bash
python3 train_cycleGAN.py \
    --domainA_dir "./data/processed/colon/ct" \ 
    --domainB_dir "./data/processed/colon/histo" \
    --color_mode "rgb" \
    --epochs 100 \
    --summary_interval 10 \
    --output_dir "./training_output"
```
Get the full list of arguments by 
```bash
python3 train_cycleGAN.py --help
```


### Elastic Registration

Run the elastic registration script to register target histology images against a reference:

```bash
python elastic_registation.py \
    --fixed "path/to/fixed_image.png" \
    --moving "path/to/moving_image.png" \
    --output_dir "./outputs/registration" \
    --max_iter 100 \
    --create_checkers 50 \
    --create_lines 50 
```
Get the full list of arguments by 
```bash
python3 elastic_registration.py --help
```

### Interactive Notebooks

Launch Jupyter to explore and run the provided notebooks:

```bash
jupyter notebook
```

Navigate to `notebooks/cycleGAN.ipynb` or `notebooks/pix2pix.ipynb` to step through data exploration, training visualizations, and style evaluation.

---

## License

This project is licensed under the terms specified in the [LICENSE](LICENSE) file.