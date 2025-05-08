# vinci_conda

A GitHub Actions workflow to create zipped Conda environments for Windows machines. You can use the created conda environment in air-gap environment by uploading them through VINCI upload tool found in VINCI Central.

## Features

- Creates/updates Conda environments from environment.yml files
- Supports multiple environment configurations in different folders
- Downloads and caches dependencies including:
  - Python packages via pip/conda
  - Spark NLP jars (optional)
  - Spacy language models (optional)
- Compresses environments into 7z archives for easy uploading
- Creates GitHub releases with built environments

## How to

1. Place your environment.yml file in a subfolder (e.g., win_dev, win_pip) or make a new folder
2. Trigger the workflow manually through GitHub Actions
3. Configure the build parameters:
   - `build_folder`: Folder containing the environment.yml (e.g., win_dev)
   - `target_folder`: Destination path for the Conda environment
   - `download_jars`: Enable Spark NLP jar downloads
   - `sparknlp_version`: Specify Spark NLP version
   - `download_spacy_model`: Download Spacy language models
   - `zip_vol_size`: Control archive volume size
   - `retention_days`: Artifact retention period
   
**Note**: if you don't have a D drive on your VINCI machine, then you might need to make some changes manually once you unzip the environment folder inside VINCI.
In File Explorer, go to [your_env_folder]\share\jupyter\kernels\python3\kernel.json and open with Notepad++. Change line 3 to  [your_env_folder]\\python.exe, save, and reactivate the environment. The sys.path in jupyter notebooks should now point towards your copy of the anaconda environment. 

In most recent version of jupyter, you might also need to change these files also. Open Notepad++, press Ctrl+Shift+F. Replace string “D:\conda_envs\win_dev” (or different folder name if you build the environment using a different github repo folder) within all the files (including exe files) under “[your_env_folder]\Scripts”  with your [your_env_folder].

## Available Environments

- `win_dev`: Basic Python development environment with Jupyter
p- `test_env`: A Python environment for demonstration.

## Installation

1. Download the 7z archives from the latest release
2. Extract to your specified target folder
3. Activate the environment using Conda
4. If your working machine does not have conda installed, you will need to install them (doesn't require admin right). Here are the [latest miniforge](https://conda-forge.org/download/)
