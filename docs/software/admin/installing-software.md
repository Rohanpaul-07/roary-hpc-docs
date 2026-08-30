# Installing Your Own Software

<div class="install-software-hero">

  <div class="install-software-hero-content">

    <span class="roary-page-eyebrow">
      SOFTWARE
    </span>

    <h2>Installing Software in Your Roary Account</h2>

    <p>
      If software is not already available on Roary, users may install
      applications inside their own account without administrator access.
    </p>

    <p>
      First check the module system. If the software is already provided
      centrally, use the Roary module instead of installing another copy.
    </p>

  </div>

  <div class="install-software-hero-badge">
    <span>ROARY</span>
    <strong>INSTALL</strong>
    <small>SOFTWARE</small>
  </div>

</div>


## Check Roary First

Before installing anything:

```bash
module avail SOFTWARE_NAME
```

For example:

```bash
module avail gcc
module avail openmpi
module avail miniconda
module avail cuda
```

If the software already exists, load the provided module:

```bash
module load SOFTWARE/VERSION
```

[Environment Modules :material-arrow-right:](../modules/index.md){ .md-button }


## Recommended Installation Options

For software not already available on Roary:

| Method | Best For |
|---|---|
| Conda / Miniconda | Python, bioinformatics, scientific packages |
| Apptainer | Docker/container-based applications |
| Source installation | Software that must be compiled manually |

---

## Option 1 — Conda / Miniconda

For many scientific applications, Conda is the easiest option.

Load Miniconda:

```bash
module avail miniconda
module load miniconda/VERSION
```

Prepare Conda:

```bash
source "$(conda info --base)/etc/profile.d/conda.sh"
```

Create an environment:

```bash
mkdir -p "$HOME/conda-envs"

conda create \
  --prefix "$HOME/conda-envs/mysoftware" \
  --channel conda-forge \
  --override-channels \
  SOFTWARE_NAME
```

Activate it:

```bash
conda activate "$HOME/conda-envs/mysoftware"
```

[Conda / Miniconda :material-arrow-right:](../languages/conda.md){ .md-button .md-button--primary }

---

## Option 2 — Apptainer

If software is distributed as a Docker or container image, use Apptainer.

Example:

```bash
apptainer pull software.sif docker://IMAGE
```

Run it:

```bash
apptainer exec software.sif COMMAND
```

For NVIDIA GPU containers:

```bash
apptainer exec --nv software.sif COMMAND
```

[Apptainer :material-arrow-right:](../containers/apptainer.md){ .md-button }

---

## Option 3 — Install from Source

Personal software should normally be installed under:

```text
$HOME/software/
```

Create the directory:

```bash
mkdir -p "$HOME/software"
```

A common source build looks like:

```bash
tar -xf software.tar.gz
cd software
```

Configure the installation location:

```bash
./configure --prefix="$HOME/software/mysoftware"
```

Build:

```bash
make
```

Install:

```bash
make install
```

The application may then be located under:

```text
$HOME/software/mysoftware/bin/
```

Add it to your current shell if needed:

```bash
export PATH="$HOME/software/mysoftware/bin:$PATH"
```

Verify:

```bash
which PROGRAM
PROGRAM --version
```

---

## CMake Software

For software using CMake:

```bash
mkdir build
cd build
```

Configure:

```bash
cmake \
  -DCMAKE_INSTALL_PREFIX="$HOME/software/mysoftware" \
  ..
```

Build:

```bash
cmake --build .
```

Install:

```bash
cmake --install .
```

---

## Do Not Use `sudo`

Users should never try to install software into system directories.

Do not use:

```bash
sudo make install
```

or:

```bash
sudo dnf install ...
```

or:

```bash
sudo yum install ...
```

Install into directories owned by your account instead.

---

## Large Software Builds

Small compilations can be performed on a login node.

Large builds that use significant CPU or memory should use an interactive
compute allocation.

Example:

```bash
salloc \
  --cpus-per-task=8 \
  --mem=16G \
  --time=02:00:00
```

Then build using the allocated CPUs:

```bash
make -j$SLURM_CPUS_PER_TASK
```

Exit when finished:

```bash
exit
```

[Running Jobs :material-arrow-right:](../../running-jobs/index.md){ .md-button }

---

## Compilers and Dependencies

Check available compilers before building software:

```bash
module avail gcc
```

or:

```bash
module avail intel
```

Load the required compiler:

```bash
module load gcc/VERSION
```

For MPI software:

```bash
module avail openmpi
```

For CUDA software:

```bash
module avail cuda
```

Use the compiler and libraries required by the application's installation
instructions.

---

## Where Should Software Be Stored?

A simple organization is:

```text
$HOME/
├── software/
│   ├── application1/
│   └── application2/
│
├── conda-envs/
│
└── projects/
```

Keep persistent software under your home storage.

Use `/scratch` primarily for temporary working data and temporary build files.

---

## When Should I Request Software Instead?

Do **not** spend significant time maintaining a personal installation if the
software:

- Will be used by many Roary users
- Requires administrator privileges
- Requires complex system dependencies
- Needs special MPI or CUDA integration
- Is difficult to build or maintain
- Would be better provided as a shared Environment Module

In those cases, submit a software request instead.

[Request New Software :material-arrow-right:](request-software.md){ .md-button .md-button--primary }

---

## Common Problems

### `Permission denied`

Make sure you are installing under a directory owned by your account:

```bash
ls -ld "$HOME/software"
```

Do not install into:

```text
/usr
/usr/local
/opt
```

### `command not found`

Check whether the executable exists:

```bash
find "$HOME/software" -type f -name PROGRAM 2>/dev/null
```

Then add its `bin` directory to your `PATH`:

```bash
export PATH="$HOME/software/mysoftware/bin:$PATH"
```

### Missing Compiler

Check available modules:

```bash
module avail gcc
module avail intel
```

### Missing Library

Check:

```bash
ldd ./PROGRAM
```

and:

```bash
module list
```

The application may require the same compiler or library modules used during
the build.

---

## Best Practices

- Check `module avail` before installing software yourself.
- Prefer Roary modules when software is already centrally available.
- Use Conda for many scientific and Python-based applications.
- Use Apptainer for containerized software.
- Install personal source builds under `$HOME/software`.
- Never use `sudo`.
- Use compute nodes for large builds.
- Record compiler and software versions.
- Request a shared installation when software is complex or broadly needed.

---

## Related Guides

[Environment Modules](../modules/index.md){ .md-button }

[Conda / Miniconda](../languages/conda.md){ .md-button }

[Apptainer](../containers/apptainer.md){ .md-button }

[Request New Software](request-software.md){ .md-button .md-button--primary }
