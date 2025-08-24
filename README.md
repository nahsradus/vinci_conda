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


## How to Fork, Set Up, and Run Workflows

### Forking and Setting Up the Repo

1. On GitHub, click the "Fork" button at the top right of this repository page to create your own copy.
2. Clone your fork to your local machine:
  ```bash
  git clone https://github.com/<your-username>/vinci_conda.git
  cd vinci_conda
  ```
3. (Optional) Set up your upstream remote to keep your fork up to date:
  ```bash
  git remote add upstream https://github.com/department-of-veterans-affairs/vinci_conda.git
  git fetch upstream
  git merge upstream/main
  ```

### Running the Workflows

#### If you have direct commit access to `main` (e.g., in your fork):

Use the `build_with_main_commit` workflow in `.github/workflows/direct-bump.yml`.

1. Go to the "Actions" tab in your forked repo.
2. Select the `build_with_main_commit` workflow (direct bump version).
3. Click "Run workflow" and fill in the required parameters (see below).
4. The workflow will bump the version and commit directly to your `main` branch, then build and release the environment.

#### If you do NOT have direct commit access to `main` (e.g., in the VA organizational repo):

Use the standard `build_win_env` workflow in `.github/workflows/build.yml`.

1. Go to the "Actions" tab in the original repo.
2. Select the `build_win_env` workflow.
3. Click "Run workflow" and fill in the required parameters.



5. Before running the workflow, you must set up a GitHub secret named `MY_PAT` in your repository settings. This should be a Personal Access Token (PAT) with `repo` and `workflow` permissions. The workflow uses this token to create branches, commits, and pull requests.

  ##### How to Create a GitHub Personal Access Token (PAT)
  1. Go to your GitHub account's token settings:  
    https://github.com/settings/tokens
  2. Click **"Generate new token"** (classic) or **"Generate new token"** (fine-grained).
  3. Give your token a name.
  4. Select the following scopes:
    - `repo` (Full control of private repositories)
    - `workflow` (Update GitHub Action workflows)
  5. Click **"Generate token"** and copy the token value.  
    **Note:** You will not be able to see it again after you leave the page.

  Official GitHub documentation:  
  https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token

  ##### How to Set the PAT as a Secret in Your Repository
  1. Go to your repository on GitHub.
  2. Open the repository secrets page:  
    `https://github.com/<your-username>/<your-repo>/settings/secrets/actions`  
    (Replace `<your-username>` and `<your-repo>` with your actual GitHub username and repository name.)
  3. Click **"New repository secret"**.
  4. Name the secret `MY_PAT`.
  5. Paste your Personal Access Token and click **"Add secret"**.

  Official GitHub documentation:  
  https://docs.github.com/en/actions/security-guides/encrypted-secrets

### Workflow Parameters

- `build_folder`: Folder containing the environment.yml (e.g., win_dev)
- `target_folder`: Destination path for the Conda environment
- `download_jars`: Enable Spark NLP jar downloads
- `sparknlp_version`: Specify Spark NLP version
- `download_spacy_model`: Download Spacy language models
- `zip_vol_size`: Control archive volume size
- `retention_days`: Artifact retention period

**Note**: If you don't have a D drive on your VINCI machine, you may need to manually update paths after extracting the environment. See below for details.

In File Explorer, go to `[your_env_folder]\share\jupyter\kernels\python3\kernel.json` and open with Notepad++. Change line 3 to `[your_env_folder]\\python.exe`, save, and reactivate the environment. The sys.path in Jupyter notebooks should now point to your copy of the Anaconda environment.

In recent versions of Jupyter, you may also need to update paths in other files. Use Notepad++ (Ctrl+Shift+F) to replace `D:\conda_envs\win_dev` (or your folder name) in all files under `[your_env_folder]\Scripts` with your `[your_env_folder]`.

## Available Environments

- `win_dev`: Basic Python development environment with Jupyter
p- `test_env`: A Python environment for demonstration.

## Installation

1. Download the 7z archives from the latest release
2. Extract to your specified target folder
3. Activate the environment using Conda
4. If your working machine does not have conda installed, you will need to install them (doesn't require admin right). Here are the [latest miniforge](https://conda-forge.org/download/)
