# MONAI Auto3DSeg Environment Setup

This document describes how to reproduce the `monai-auto3dseg` conda environment used for medical image analysis and 3D segmentation tasks.

## Environment Files

Three environment specification files are provided:

1. **`monai-auto3dseg-environment.yml`** - Complete environment with exact versions (most precise)
2. **`monai-auto3dseg-portable.yml`** - Portable environment without build numbers (more flexible)
3. **`monai-auto3dseg-requirements.txt`** - Pip requirements file (fallback option)

## Recommended Setup Method

### Method 1: Full Environment Recreation (Recommended)
```bash
# Create environment from exact specification
conda env create -f monai-auto3dseg-environment.yml

# Activate the environment
conda activate monai-auto3dseg
```

### Method 2: Portable Setup (Cross-platform)
```bash
# Create environment from portable specification
conda env create -f monai-auto3dseg-portable.yml

# Activate the environment  
conda activate monai-auto3dseg
```

### Method 3: Manual Setup with Pip
```bash
# Create new environment with Python 3.9
conda create -n monai-auto3dseg python=3.9

# Activate environment
conda activate monai-auto3dseg

# Install packages from pip requirements
pip install -r monai-auto3dseg-requirements.txt
```

## Key Dependencies

This environment includes:
- **MONAI** - Medical image analysis framework
- **PyTorch** - Deep learning framework with CUDA support
- **SimpleITK** - Medical image I/O and processing
- **NumPy, Pandas** - Data manipulation
- **Matplotlib, Seaborn** - Visualization
- **Scikit-learn** - Machine learning utilities
- **Jupyter** - Interactive development

## SLURM Integration

For HPC environments using SLURM, use this template:

```bash
#!/bin/bash
#SBATCH --job-name=my_job
#SBATCH --mail-type=END,FAIL    
#SBATCH --mail-user=your_email@duke.edu
#SBATCH --ntasks=1
#SBATCH --gpus=1
#SBATCH --cpus-per-task=8
#SBATCH --output=logs/output-%j.out
#SBATCH --error=logs/error-%j.out

echo "Job starting"
echo "GPUs Given: $CUDA_VISIBLE_DEVICES"

# Load miniconda module (adjust for your system)
module load miniconda/py39_4.12.0

# Activate the environment
source activate monai-auto3dseg

# Your python script here
python your_script.py
```

## Troubleshooting

### If environment creation fails:
1. Try the portable version first
2. Update conda: `conda update conda`
3. Clear conda cache: `conda clean --all`
4. Use mamba for faster resolution: `mamba env create -f monai-auto3dseg-environment.yml`

### For CUDA issues:
- Ensure CUDA drivers are compatible with PyTorch version
- Check `nvidia-smi` output matches PyTorch CUDA version
- May need to reinstall PyTorch with specific CUDA version

### For missing packages:
- Install missing packages: `conda install package_name`
- Or use pip: `pip install package_name`

## Environment Info

- **Python Version**: 3.9.x
- **Primary Use Case**: Medical image analysis, 3D segmentation, deep learning
- **GPU Support**: CUDA-enabled PyTorch
- **Created**: September 2025
- **Platform**: Linux x86_64 (tested on SLURM HPC)

## Sharing and Distribution

To share this environment:

1. **GitHub Repository**: Commit all `.yml` and `.txt` files
2. **Docker**: Create Dockerfile based on environment specs
3. **Conda-pack**: Create portable environment archive
4. **Documentation**: Include this README for setup instructions

## Updates

To update the environment specifications:

```bash
# Activate environment
conda activate monai-auto3dseg

# Export updated environment
conda env export > monai-auto3dseg-environment.yml
conda env export --no-builds > monai-auto3dseg-portable.yml
pip freeze > monai-auto3dseg-requirements.txt
```