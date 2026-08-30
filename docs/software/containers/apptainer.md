# Apptainer

<div class="apptainer-hero">

  <div class="apptainer-hero-content">

    <span class="roary-page-eyebrow">
      CONTAINERS
    </span>

    <h2>Apptainer on Roary</h2>

    <p>
      Apptainer is the modern continuation of the Singularity container
      technology commonly used on HPC systems.
    </p>

    <p>
      <strong>Panther used Singularity, while Roary uses Apptainer.</strong>
      The commands and container format are very similar, so most users
      migrating from Panther will find the workflow familiar.
    </p>

  </div>

  <div class="apptainer-hero-badge">
    <span>ROARY</span>
    <strong>APPTAINER</strong>
    <small>CONTAINERS</small>
  </div>

</div>


## Singularity to Apptainer

If you previously used Singularity on Panther, the transition to Apptainer on
Roary is straightforward.

The basic workflow remains:

```text
Container Image
      ↓
Apptainer
      ↓
Slurm Compute Node
      ↓
Application
```

Many Singularity `.sif` images and workflows can continue to be used with
Apptainer.

For example, a Panther command such as:

```bash
singularity exec container.sif python analysis.py
```

becomes:

```bash
apptainer exec container.sif python analysis.py
```

!!! info "Panther → Roary"
    **Panther:** Singularity

    **Roary:** Apptainer

    Apptainer is the container platform users should use for new Roary
    workflows.


## Quick Start

Check whether Apptainer is available:

```bash
module avail apptainer
```

Load it if a module is required:

```bash
module load apptainer/VERSION
```

Verify:

```bash
which apptainer
apptainer --version
```

Run a container:

```bash
apptainer exec container.sif COMMAND
```

Open a shell inside a container:

```bash
apptainer shell container.sif
```


## What Is Apptainer?

Apptainer allows applications and their dependencies to be packaged into a
portable container image.

A container can include:

- Applications
- Python environments
- Libraries
- System utilities
- Scientific software
- Workflow dependencies

This is useful when software has complex dependencies or when the same
environment must be reproduced across systems.

Unlike traditional Docker workflows, Apptainer is designed for shared HPC
environments and does not require users to run containers as root.


## Container Images

The common Apptainer image format is:

```text
.sif
```

For example:

```text
mysoftware.sif
```

A `.sif` image is normally a single file, which makes it convenient to store,
copy, and use on an HPC cluster.


## Pull a Container

Apptainer can pull images from container registries.

For example, pull an Ubuntu image:

```bash
apptainer pull ubuntu.sif docker://ubuntu:24.04
```

Pull a Python image:

```bash
apptainer pull python.sif docker://python:3.12
```

Then verify:

```bash
ls -lh *.sif
```

Run a command:

```bash
apptainer exec python.sif python --version
```

!!! tip
    Use specific image tags rather than `latest` when reproducibility is
    important.


## Run a Container

Run the default container command:

```bash
apptainer run container.sif
```

Run a specific command:

```bash
apptainer exec container.sif COMMAND
```

For example:

```bash
apptainer exec python.sif python --version
```

or:

```bash
apptainer exec python.sif python analysis.py
```


## Open an Interactive Shell

To enter the container:

```bash
apptainer shell container.sif
```

You will receive a shell inside the container environment.

Check:

```bash
which python
python --version
```

Exit when finished:

```bash
exit
```


## Accessing Roary Files

Apptainer containers can access host filesystems through bind mounts.

Your current working directory is commonly available inside the container.

For explicit access, use:

```bash
apptainer exec \
  --bind /path/on/roary:/data \
  container.sif \
  COMMAND
```

For example:

```bash
apptainer exec \
  --bind "$HOME/project:/project" \
  container.sif \
  python /project/analysis.py
```

Inside the container:

```text
/project
```

points to:

```text
$HOME/project
```

on Roary.


## Working with Scratch Storage

Large temporary container workloads should use scratch storage when
appropriate.

For example:

```bash
apptainer exec \
  --bind /scratch/USERNAME/project:/work \
  container.sif \
  COMMAND
```

Then the application can use:

```text
/work
```

inside the container.

!!! warning
    Scratch is temporary storage.

    Keep important scripts, container definitions, and persistent results in
    appropriate persistent storage.


## Apptainer Cache

Container pulls can create a local cache.

Check the default cache:

```bash
apptainer cache list
```

For large container workflows, you may prefer to place the cache in scratch:

```bash
mkdir -p /scratch/$USER/apptainer-cache

export APPTAINER_CACHEDIR=/scratch/$USER/apptainer-cache
```

Then pull the image:

```bash
apptainer pull container.sif docker://IMAGE
```

Clean unused cache files:

```bash
apptainer cache clean
```

!!! tip
    Using scratch for large temporary container caches can help reduce home
    storage usage.


## Using Apptainer with Slurm

Containers do **not** replace Slurm.

Slurm still controls:

- CPUs
- Memory
- GPUs
- Compute nodes
- Runtime
- Partitions
- QOS

A normal Apptainer Slurm job looks like:

```bash
#!/bin/bash

#SBATCH --job-name=apptainer-job
#SBATCH --output=apptainer_%j.out
#SBATCH --error=apptainer_%j.err
#SBATCH --cpus-per-task=4
#SBATCH --mem=8G
#SBATCH --time=01:00:00

module purge
module load apptainer/VERSION

echo "======================================"
echo "Job ID     : $SLURM_JOB_ID"
echo "Node       : $(hostname)"
echo "Apptainer  : $(apptainer --version)"
echo "======================================"

apptainer exec \
  container.sif \
  python analysis.py
```

Submit:

```bash
sbatch job.sh
```

!!! important
    Computational container workloads should run on compute nodes through
    Slurm, not directly on login nodes.

[Running Jobs :material-arrow-right:](../../running-jobs/index.md){ .md-button .md-button--primary }


## GPU Containers

Apptainer can expose NVIDIA GPUs to a container using:

```bash
--nv
```

For example:

```bash
apptainer exec --nv container.sif nvidia-smi
```

For a GPU application:

```bash
apptainer exec --nv container.sif python gpu_program.py
```

A Slurm GPU job may look like:

```bash
#!/bin/bash

#SBATCH --job-name=apptainer-gpu
#SBATCH --output=apptainer_gpu_%j.out
#SBATCH --error=apptainer_gpu_%j.err

#SBATCH --partition=GPU_PARTITION
#SBATCH --qos=GPU_QOS
#SBATCH --gres=gpu:1

#SBATCH --cpus-per-task=4
#SBATCH --mem=16G
#SBATCH --time=01:00:00

module purge
module load apptainer/VERSION

nvidia-smi

apptainer exec \
  --nv \
  container.sif \
  python gpu_program.py
```

!!! important
    `--nv` makes the allocated NVIDIA GPU and required host libraries
    available inside the container.

    It does **not** allocate a GPU by itself.

    The GPU must still be requested through Slurm.

[GPU Computing :material-arrow-right:](../../gpus/index.md){ .md-button }


## Docker Images on Roary

You do not need Docker running on the compute node to use many Docker images.

Apptainer can pull compatible images directly from Docker registries.

Example:

```bash
apptainer pull myimage.sif docker://ubuntu:24.04
```

Then:

```bash
apptainer exec myimage.sif cat /etc/os-release
```

This makes Apptainer useful when research software is distributed as a Docker
image but must run on an HPC system.


## Building Containers

Apptainer can build `.sif` images from definition files or container
registries.

Example from Docker:

```bash
apptainer build myimage.sif docker://ubuntu:24.04
```

However, some container build operations may require privileges or features
that are restricted on shared HPC systems.

For normal Roary usage, pulling an existing image is often simpler:

```bash
apptainer pull myimage.sif docker://IMAGE
```

!!! warning
    Large container builds can consume substantial CPU, storage, and temporary
    space.

    Do not run heavy container builds on shared login nodes.


## Inspect a Container

Display container metadata:

```bash
apptainer inspect container.sif
```

Show available runscript information:

```bash
apptainer inspect --runscript container.sif
```

Check the files inside interactively:

```bash
apptainer shell container.sif
```


## Environment Variables

Host environment variables may be visible inside containers.

You can explicitly define a container environment variable using:

```bash
APPTAINERENV_VARIABLE=value
```

For example:

```bash
export APPTAINERENV_OMP_NUM_THREADS=8
```

Then:

```bash
apptainer exec container.sif env | grep OMP
```

For Slurm jobs:

```bash
export APPTAINERENV_OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
```


## Common Commands

| Task | Command |
|---|---|
| Check version | `apptainer --version` |
| Pull image | `apptainer pull IMAGE.sif docker://IMAGE` |
| Run container | `apptainer run IMAGE.sif` |
| Execute command | `apptainer exec IMAGE.sif COMMAND` |
| Open shell | `apptainer shell IMAGE.sif` |
| Bind directory | `apptainer exec --bind HOST:CONTAINER IMAGE.sif COMMAND` |
| Enable NVIDIA GPU | `apptainer exec --nv IMAGE.sif COMMAND` |
| Inspect image | `apptainer inspect IMAGE.sif` |
| Show cache | `apptainer cache list` |
| Clean cache | `apptainer cache clean` |


## Common Problems

### `apptainer: command not found`

Check:

```bash
module avail apptainer
```

Load the module:

```bash
module load apptainer/VERSION
```

Verify:

```bash
which apptainer
apptainer --version
```


### Old Singularity Command No Longer Works

If an old Panther workflow contains:

```bash
singularity exec container.sif COMMAND
```

change it to:

```bash
apptainer exec container.sif COMMAND
```

Similarly:

```text
singularity run
```

becomes:

```text
apptainer run
```

and:

```text
singularity shell
```

becomes:

```text
apptainer shell
```


### Container Cannot See My Data

Bind the directory explicitly:

```bash
apptainer exec \
  --bind /path/on/roary:/data \
  container.sif \
  COMMAND
```

Then access it inside the container through:

```text
/data
```


### GPU Is Not Visible

Make sure the Slurm job requested a GPU.

Then use:

```bash
apptainer exec --nv container.sif nvidia-smi
```

Also check:

```bash
echo "$CUDA_VISIBLE_DEVICES"
```


### Container Pull Uses Too Much Home Storage

Set the cache to scratch:

```bash
mkdir -p /scratch/$USER/apptainer-cache

export APPTAINER_CACHEDIR=/scratch/$USER/apptainer-cache
```

Then retry the pull.


### Permission Denied

Remember that Apptainer containers normally run with your user permissions.

A container does not give you root privileges on Roary.

Check the host file permissions:

```bash
ls -ld /path/to/data
```

and:

```bash
ls -l /path/to/file
```


## Best Practices

- Use Apptainer for container workflows on Roary.
- Convert old Panther `singularity` commands to `apptainer`.
- Keep important `.sif` images in persistent project storage.
- Use scratch for large temporary container caches.
- Use `apptainer exec` for most batch workflows.
- Use `--bind` when explicit filesystem access is required.
- Use `--nv` for NVIDIA GPU containers.
- Request GPUs through Slurm; `--nv` alone does not allocate a GPU.
- Do not run heavy container workloads or builds on login nodes.
- Use specific container image versions or tags for reproducibility.
- Keep container commands inside Slurm job scripts for reproducible workflows.


## Need Help?

Collect:

```bash
hostname
module list

which apptainer
apptainer --version
```

For the container:

```bash
apptainer inspect container.sif
```

For GPU problems:

```bash
nvidia-smi
echo "$CUDA_VISIBLE_DEVICES"
```

For Slurm problems, also provide:

- Job ID
- Job script
- Output file
- Error file
- Container image name
- Exact Apptainer command

For assistance:

[**hpcadmin@fiu.edu**](mailto:hpcadmin@fiu.edu)


## Related Guides

[Environment Modules](../modules/index.md){ .md-button }

[Conda / Miniconda](../languages/conda.md){ .md-button }

[CUDA](../parallel/cuda.md){ .md-button }

[GPU Computing](../../gpus/index.md){ .md-button }

[Running Jobs](../../running-jobs/index.md){ .md-button }
