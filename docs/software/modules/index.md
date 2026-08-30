# Environment Modules

<div class="module-hero">

  <div class="module-hero-content">

    <span class="roary-page-eyebrow">
      ROARY SOFTWARE ENVIRONMENT
    </span>

    <h2>
      Find and load software on Roary
    </h2>

    <p>
      Roary uses Environment Modules to provide centrally installed
      applications, compilers, libraries, MPI implementations, CUDA
      toolkits, and other research software.
    </p>

    <p>
      Modules allow multiple software versions to coexist while letting
      you choose the version required for your workflow.
    </p>

  </div>

  <div class="module-hero-badge">
    <span>ROARY</span>
    <strong>MODULES</strong>
    <small>SOFTWARE</small>
  </div>

</div>


## Quick Start

See available software:

```bash
module avail
```

Find a particular application:

```bash
module avail SOFTWARE
```

For example:

```bash
module avail miniconda
module avail openmpi
module avail gcc
module avail cuda
```

Load software:

```bash
module load SOFTWARE/VERSION
```

See what is currently loaded:

```bash
module list
```

Remove a module:

```bash
module unload SOFTWARE
```

Remove all loaded modules:

```bash
module purge
```

!!! tip "Use module avail first"
    Software versions on Roary may change over time.

    Always check the currently available versions before loading software.

---

## Essential Module Commands

| Task | Command |
|---|---|
| List available software | `module avail` |
| Find software | `module avail NAME` |
| Load software | `module load NAME/VERSION` |
| Show loaded modules | `module list` |
| Inspect a module | `module show NAME/VERSION` |
| Unload software | `module unload NAME` |
| Remove all modules | `module purge` |

These commands are enough for most Roary users.

---

## Finding Software

To see all available modules:

```bash
module avail
```

To search for a particular application:

```bash
module avail NAME
```

Examples:

```bash
module avail miniconda
```

```bash
module avail openmpi
```

```bash
module avail gcc
```

```bash
module avail cuda
```

If the application is available, Roary will display one or more versions.

---

## Loading Software

After finding the software version you want:

```bash
module load NAME/VERSION
```

For example:

```bash
module load gcc/VERSION
```

Then verify it:

```bash
module list
```

Check which executable is being used:

```bash
which PROGRAM
```

and check its version:

```bash
PROGRAM --version
```

A useful general pattern is:

```bash
module purge
module load SOFTWARE/VERSION

module list
which PROGRAM
PROGRAM --version
```

!!! important "Use explicit versions"
    For reproducible research and Slurm jobs, prefer:

    ```bash
    module load SOFTWARE/VERSION
    ```

    rather than relying on an unspecified default version.

---

## Inspecting a Module

Before loading software, you can inspect its modulefile:

```bash
module show NAME/VERSION
```

This can show:

- Software paths
- Environment variables
- Required dependencies
- Conflicting modules
- Library paths

For example:

```bash
module show openmpi/VERSION
```

---

## Starting With a Clean Environment

If you have several modules loaded or software is behaving unexpectedly:

```bash
module purge
```

Then load only what the workflow requires:

```bash
module load SOFTWARE/VERSION
```

Check the result:

```bash
module list
```

This is one of the first troubleshooting steps for software problems.

---

## Module Dependencies and Conflicts

Some scientific software depends on other software such as:

```text
Compiler
   ↓
MPI
   ↓
Scientific Libraries
   ↓
Application
```

For example, an MPI installation may depend on a particular compiler.

Some software stacks should not be mixed.

Common examples include:

- Different GCC versions
- GCC and Intel compiler stacks
- Different MPI implementations
- Different CUDA versions
- Multiple versions of the same application

If a module reports a conflict, check:

```bash
module list
```

Then unload the conflicting module:

```bash
module unload NAME
```

or start clean:

```bash
module purge
```

!!! warning "Do not blindly mix MPI or compiler modules"
    Software compiled with one MPI or compiler stack may not work correctly
    with another.

---

## Using Modules in Slurm Jobs

Required modules should be loaded **inside the Slurm job script**.

Example:

```bash
#!/bin/bash

#SBATCH --job-name=module-example
#SBATCH --output=module_%j.out
#SBATCH --error=module_%j.err
#SBATCH --cpus-per-task=4
#SBATCH --mem=8G
#SBATCH --time=01:00:00

module purge
module load SOFTWARE/VERSION

echo "======================================"
echo "Job ID : $SLURM_JOB_ID"
echo "Node   : $(hostname)"
echo "======================================"

module list

which PROGRAM
PROGRAM --version

PROGRAM input.dat
```

Submit the job:

```bash
sbatch job.sh
```

!!! important
    Do not assume that software loaded manually on a login node will
    automatically provide the correct environment for a Slurm job.

    Load the required modules explicitly inside the job script.

[Running Jobs :material-arrow-right:](../../running-jobs/index.md){ .md-button .md-button--primary }

---

## Miniconda Example

Find Miniconda:

```bash
module avail miniconda
```

Load an available version:

```bash
module load miniconda/VERSION
```

Verify:

```bash
which conda
conda --version
```

For Conda environment management, see:

[Conda / Miniconda :material-arrow-right:](../languages/conda.md){ .md-button }

---

## OpenMPI Example

Find OpenMPI:

```bash
module avail openmpi
```

Load an available version:

```bash
module load openmpi/VERSION
```

Check the MPI tools:

```bash
which mpicc
which mpirun
```

Verify:

```bash
mpirun --version
```

The OpenMPI guide covers compiling and running MPI applications through Slurm.

[OpenMPI :material-arrow-right:](../parallel/openmpi.md){ .md-button }

---

## Common Problems

### Module Not Found

If:

```bash
module load SOFTWARE/VERSION
```

reports that the module cannot be found, check:

```bash
module avail SOFTWARE
```

Make sure the module name and version are correct.

---

### Command Not Found After Loading

Check what is loaded:

```bash
module list
```

Inspect the module:

```bash
module show SOFTWARE/VERSION
```

Check for the executable:

```bash
which PROGRAM
```

---

### Wrong Software Version

Check:

```bash
module list
```

Start clean if necessary:

```bash
module purge
```

Then load the required version:

```bash
module load SOFTWARE/VERSION
```

---

### Software Works on Login Node but Fails in Slurm

Make sure your job script contains:

```bash
module purge
module load SOFTWARE/VERSION
```

Then add:

```bash
module list
which PROGRAM
PROGRAM --version
```

to the job output while troubleshooting.

---

## Best Practices

- Use `module avail` to discover software.
- Use explicit software versions when possible.
- Use `module list` to verify your environment.
- Use `module purge` when starting a clean workflow.
- Load required modules inside Slurm scripts.
- Do not mix incompatible compiler, MPI, or CUDA stacks.
- Check whether software already exists as a Roary module before installing your own copy.
- Record important software versions with your research workflow.

---

## Need Help?

Collect the following information:

```bash
hostname
module list
module avail SOFTWARE
```

If the software is loaded:

```bash
which PROGRAM
PROGRAM --version
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

[Software Overview](../index.md){ .md-button }

[Conda / Miniconda](../languages/conda.md){ .md-button }

[OpenMPI](../parallel/openmpi.md){ .md-button }

[Running Jobs](../../running-jobs/index.md){ .md-button }
