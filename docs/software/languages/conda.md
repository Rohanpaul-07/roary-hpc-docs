# Conda / Miniconda

<div class="conda-hero">
  <div class="conda-hero-content">
    <span class="roary-page-eyebrow">LANGUAGES & ENVIRONMENTS</span>
    <h2>Conda and Miniconda on Roary</h2>
    <p>
      Create isolated and reproducible software environments for Python,
      scientific computing, bioinformatics, machine learning, and other
      research workflows.
    </p>
    <p>
      Roary provides Miniconda through the Environment Modules system.
      Users should normally use the provided module instead of installing
      their own Miniconda distribution.
    </p>
  </div>

  <div class="conda-hero-badge">
    <span>ROARY</span>
    <strong>CONDA</strong>
    <small>MINICONDA</small>
  </div>
</div>

## Quick Start

Check which Miniconda versions are available:

```bash
module avail miniconda
```

Load one of the versions shown:

```bash
module load miniconda/VERSION
```

Enable Conda environment activation in the current shell:

```bash
source "$(conda info --base)/etc/profile.d/conda.sh"
```

Create a directory for your Conda environments:

```bash
mkdir -p "$HOME/conda-envs"
```

Create an environment:

```bash
conda create \
  --prefix "$HOME/conda-envs/myproject" \
  --channel conda-forge \
  --override-channels \
  python=3.11
```

Activate it:

```bash
conda activate "$HOME/conda-envs/myproject"
```

Verify the environment:

```bash
echo "$CONDA_PREFIX"
which python
python --version
conda list
```

When finished:

```bash
conda deactivate
```

!!! tip "Check available versions first"
    Always run:

    ```bash
    module avail miniconda
    ```

    before loading Miniconda because installed versions may change over time.

---

## What Are Conda and Miniconda?

**Conda** is a package and environment manager.

It allows each research project to have its own:

- Python version
- Python packages
- Scientific libraries
- Bioinformatics applications
- Compiled dependencies
- Machine-learning packages

**Miniconda** is a minimal distribution that provides Conda and the basic
software required to create these environments.

On Roary, Miniconda is already provided through the module system.

!!! important
    You normally do **not** need to download or install your own copy of
    Miniconda or Anaconda.

---

## Using Miniconda on Roary

Find the available Miniconda modules:

```bash
module avail miniconda
```

Load a version:

```bash
module load miniconda/VERSION
```

Verify:

```bash
which conda
conda --version
```

You can also inspect where the loaded Miniconda installation is located:

```bash
conda info --base
```

For Slurm jobs and reproducible workflows, use an explicit Miniconda version
instead of relying on a default module.

[Environment Modules :material-arrow-right:](../modules/index.md){ .md-button }

---

## Where Should I Store My Environments?

Persistent Conda environments should normally be stored under your home
directory.

Recommended location:

```text
$HOME/conda-envs/
```

For example:

```text
/home/USERNAME/conda-envs/
├── genomics/
├── machine-learning/
└── project1/
```

Create the directory once:

```bash
mkdir -p "$HOME/conda-envs"
```

!!! warning "Do not keep permanent environments in scratch"
    `/scratch` is intended for temporary working data.

    An environment that you need long-term should be kept in persistent
    storage such as your home directory.

[Storage & Data :material-arrow-right:](../../storage/index.md){ .md-button }

---

## Creating an Environment

A path-based environment is recommended because its location is explicit.

Example:

```bash
conda create \
  --prefix "$HOME/conda-envs/myproject" \
  --channel conda-forge \
  --override-channels \
  python=3.11 \
  numpy \
  scipy \
  pandas \
  matplotlib
```

Activate it:

```bash
conda activate "$HOME/conda-envs/myproject"
```

Check which environment is active:

```bash
echo "$CONDA_PREFIX"
```

List all environments:

```bash
conda env list
```

List installed packages:

```bash
conda list
```

---

## Installing Packages

Activate the environment first:

```bash
conda activate "$HOME/conda-envs/myproject"
```

For general scientific software, `conda-forge` is commonly used:

```bash
conda install \
  --channel conda-forge \
  --override-channels \
  numpy scipy pandas scikit-learn
```

To search for a package:

```bash
conda search \
  --channel conda-forge \
  --override-channels \
  PACKAGE
```

Before installing software yourself, also check whether Roary already provides
it as a module:

```bash
module avail SOFTWARE_NAME
```

A centrally installed Roary module may be preferable for large or commonly
used HPC applications.

---

## Bioinformatics with Bioconda

Bioinformatics software is commonly available through Bioconda.

Example:

```bash
conda create \
  --prefix "$HOME/conda-envs/genomics" \
  --channel conda-forge \
  --channel bioconda \
  --override-channels \
  --strict-channel-priority \
  samtools \
  bcftools \
  bwa
```

Activate it:

```bash
conda activate "$HOME/conda-envs/genomics"
```

Verify the software:

```bash
samtools --version
bcftools --version
```

---

## Using Conda with pip

Some Python packages may only be available through `pip`.

Install as much as possible with Conda first:

```bash
conda install \
  --channel conda-forge \
  --override-channels \
  pip numpy scipy pandas
```

Then install pip-only packages:

```bash
python -m pip install PACKAGE
```

Verify that pip belongs to the active environment:

```bash
which python
python -m pip --version
```

!!! warning "Do not use pip --user inside Conda"
    Avoid:

    ```bash
    python -m pip install --user PACKAGE
    ```

    This installs packages outside the active Conda environment and can
    create conflicts.

---

## Do Not Modify the Shared Base Environment

The Miniconda module is centrally managed by the Roary HPC team.

Do **not** install packages into the shared base environment:

```bash
conda install -n base PACKAGE
```

Do **not** attempt to update the shared Conda installation:

```bash
conda update conda
```

Do **not** use:

```bash
sudo conda ...
```

Create your own environment instead.

---

## Avoid `conda init`

On personal computers, Conda may recommend:

```bash
conda init
```

On Roary, it is better to load Conda only when needed.

Use:

```bash
module load miniconda/VERSION
source "$(conda info --base)/etc/profile.d/conda.sh"
```

This avoids permanently modifying your shell startup configuration and keeps
the module environment easier to understand.

---

## Using Conda in Slurm Jobs

Always load Miniconda and activate the required environment **inside the Slurm
job script**.

Example:

```bash
#!/bin/bash

#SBATCH --job-name=conda-job
#SBATCH --output=conda_%j.out
#SBATCH --error=conda_%j.err
#SBATCH --cpus-per-task=4
#SBATCH --mem=8G
#SBATCH --time=01:00:00

module purge
module load miniconda/VERSION

source "$(conda info --base)/etc/profile.d/conda.sh"

conda activate "$HOME/conda-envs/myproject"

echo "======================================"
echo "Job ID       : $SLURM_JOB_ID"
echo "Node         : $(hostname)"
echo "Conda env    : $CONDA_PREFIX"
echo "Python       : $(which python)"
echo "Python ver.  : $(python --version 2>&1)"
echo "======================================"

python analysis.py
```

Replace `VERSION` with a version shown by:

```bash
module avail miniconda
```

Submit the job:

```bash
sbatch job.sh
```

!!! important
    Do not assume that a Conda environment activated on the login node will
    automatically be active inside a Slurm job.

[Running Jobs :material-arrow-right:](../../running-jobs/index.md){ .md-button .md-button--primary }

---

## Conda and Jupyter

A Conda environment can be registered as a Jupyter kernel.

Load Miniconda and activate the environment:

```bash
module load miniconda/VERSION
source "$(conda info --base)/etc/profile.d/conda.sh"

conda activate "$HOME/conda-envs/myproject"
```

Install `ipykernel`:

```bash
conda install \
  --channel conda-forge \
  --override-channels \
  ipykernel
```

Register the environment:

```bash
python -m ipykernel install \
  --user \
  --name myproject \
  --display-name "Python (myproject)"
```

List available kernels:

```bash
jupyter kernelspec list
```

The environment can then be selected from Jupyter.

[Jupyter :material-arrow-right:](../interactive/jupyter.md){ .md-button }

---

## Saving an Environment

For reproducible research, save the software requirements with your project.

Export the environment:

```bash
conda env export \
  --prefix "$HOME/conda-envs/myproject" \
  --from-history > environment.yml
```

Example `environment.yml`:

```yaml
name: myproject

channels:
  - conda-forge

dependencies:
  - python=3.11
  - numpy
  - scipy
  - pandas
```

Recreate the environment later:

```bash
conda env create \
  --prefix "$HOME/conda-envs/myproject" \
  --file environment.yml
```

Keep `environment.yml` with your project files or source-code repository.

---

## Removing an Environment

Deactivate the environment:

```bash
conda deactivate
```

Remove it:

```bash
conda remove \
  --prefix "$HOME/conda-envs/myproject" \
  --all
```

Always verify the path before removing an environment.

---

## Storage and Package Cache

Conda environments may contain many thousands of files.

Check environment usage:

```bash
du -sh "$HOME/conda-envs" 2>/dev/null
```

Check Conda's user files:

```bash
du -sh "$HOME/.conda" 2>/dev/null
```

Check your Roary quota:

```bash
myquota
```

Clean unused Conda caches when necessary:

```bash
conda clean --all
```

Review what Conda plans to remove before confirming.

---

## Common Problems

### `conda: command not found`

Check and load Miniconda:

```bash
module avail miniconda
module load miniconda/VERSION
```

Verify:

```bash
which conda
conda --version
```

### `conda activate` does not work

Prepare Conda for the current shell:

```bash
source "$(conda info --base)/etc/profile.d/conda.sh"
```

Then activate the environment:

```bash
conda activate "$HOME/conda-envs/myproject"
```

### `NoWritablePkgsDirError`

Create a package cache owned by your account:

```bash
mkdir -p "$HOME/.conda/pkgs"
export CONDA_PKGS_DIRS="$HOME/.conda/pkgs"
```

Then retry the Conda command.

### `PackagesNotFoundError`

Search the appropriate channel:

```bash
conda search \
  --channel conda-forge \
  --override-channels \
  PACKAGE
```

For bioinformatics:

```bash
conda search \
  --channel conda-forge \
  --channel bioconda \
  --override-channels \
  PACKAGE
```

### Network or proxy errors

If you see errors such as:

```text
ProxyError
CondaHTTPError
Connection timed out
Tunnel connection failed
```

collect:

```bash
hostname
env | grep -i proxy
conda config --show-sources
```

Do not disable SSL verification as a workaround.

Contact the HPC Admins if the problem continues.

---

## Useful Commands

| Task | Command |
|---|---|
| Find Miniconda | `module avail miniconda` |
| Load Miniconda | `module load miniconda/VERSION` |
| Activate environment | `conda activate PATH` |
| Deactivate environment | `conda deactivate` |
| List environments | `conda env list` |
| List packages | `conda list` |
| Install package | `conda install PACKAGE` |
| Remove package | `conda remove PACKAGE` |
| Show Conda information | `conda info` |
| Export environment | `conda env export --from-history` |
| Clean package cache | `conda clean --all` |

---

## Best Practices

- Use one Conda environment per project.
- Use Roary's Miniconda module instead of installing another copy.
- Keep persistent Conda environments under `$HOME`.
- Use an explicit Miniconda module version in Slurm jobs.
- Install packages with Conda before using `pip`.
- Keep an `environment.yml` with important projects.
- Activate the environment inside every Slurm job.
- Check `module avail` before installing software already available on Roary.
- Do computational work on compute nodes rather than login nodes.

---

## Need Help?

Before contacting support, collect:

```bash
hostname
module list
which conda
conda --version
conda info --envs
echo "$CONDA_PREFIX"
```

For Slurm problems, also include:

- Job ID
- Job script
- Output file
- Error file

For assistance:

[**hpcadmin@fiu.edu**](mailto:hpcadmin@fiu.edu)

---

## Related Guides

[Python](python.md){ .md-button }

[Environment Modules](../modules/index.md){ .md-button }

[Jupyter](../interactive/jupyter.md){ .md-button }

[Storage & Data](../../storage/index.md){ .md-button }

[Running Jobs](../../running-jobs/index.md){ .md-button }
