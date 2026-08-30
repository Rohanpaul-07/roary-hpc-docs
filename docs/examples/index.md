# Job Examples

<div class="examples-hero">

  <div class="examples-hero-content">

    <span class="roary-page-eyebrow">
      ROARY EXAMPLES
    </span>

    <h2>Common Slurm Job Examples</h2>

    <p>
      These examples provide simple starting points for common Roary
      workloads. Copy a template, adjust the requested resources, load
      the software you need, and submit it with Slurm.
    </p>

  </div>

  <div class="examples-hero-badge">
    <span>ROARY</span>
    <strong>JOBS</strong>
    <small>EXAMPLES</small>
  </div>

</div>


## Before You Submit

Create a directory for job logs:

```bash
mkdir -p logs
```

Submit a job with:

```bash
sbatch job.sh
```

Check your jobs:

```bash
squeue -u "$USER"
```

After submission, Slurm returns a job ID such as:

```text
Submitted batch job 123456
```

You can use that job ID with:

```bash
/home/share/bin/jobusage 123456
```

For live monitoring:

```bash
/home/share/bin/jobusage -w 123456
```


## Basic CPU Job

Use this for a normal single-process application.

```bash
#!/bin/bash

#SBATCH --job-name=cpu-job
#SBATCH --output=logs/cpu_%j.out
#SBATCH --error=logs/cpu_%j.err

#SBATCH --cpus-per-task=1
#SBATCH --mem=4G
#SBATCH --time=01:00:00

echo "======================================"
echo "Job ID : $SLURM_JOB_ID"
echo "Node   : $(hostname)"
echo "Start  : $(date)"
echo "======================================"

./my_program

echo "End    : $(date)"
```

Submit:

```bash
sbatch job.sh
```


## Multicore / OpenMP Job

Use this when the application supports multiple CPU threads.

```bash
#!/bin/bash

#SBATCH --job-name=openmp-job
#SBATCH --output=logs/openmp_%j.out
#SBATCH --error=logs/openmp_%j.err

#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=8

#SBATCH --mem=8G
#SBATCH --time=01:00:00

module purge
module load gcc/VERSION

export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
export OMP_PLACES=cores
export OMP_PROC_BIND=close

echo "Node: $(hostname)"
echo "OpenMP threads: $OMP_NUM_THREADS"

./my_openmp_program
```

For OpenMP jobs:

```text
--ntasks=1
--cpus-per-task=NUMBER_OF_THREADS
```

[OpenMP :material-arrow-right:](../software/parallel/openmp.md){ .md-button }


## MPI Job

Use MPI when an application needs multiple processes or multiple compute
nodes.

Example using 2 nodes and 16 MPI ranks:

```bash
#!/bin/bash

#SBATCH --job-name=mpi-job
#SBATCH --output=logs/mpi_%j.out
#SBATCH --error=logs/mpi_%j.err

#SBATCH --nodes=2
#SBATCH --ntasks=16
#SBATCH --ntasks-per-node=8

#SBATCH --mem=16G
#SBATCH --time=01:00:00

module purge
module load openmpi/VERSION

echo "======================================"
echo "Job ID    : $SLURM_JOB_ID"
echo "Nodes     : $SLURM_JOB_NUM_NODES"
echo "MPI tasks : $SLURM_NTASKS"
echo "Node list : $SLURM_NODELIST"
echo "======================================"

srun ./my_mpi_program
```

For a normal MPI workload:

```text
1 Slurm task = 1 MPI rank
```

[OpenMPI :material-arrow-right:](../software/parallel/openmpi.md){ .md-button }


## Conda Job

Use this when your application is installed inside a Conda environment.

```bash
#!/bin/bash

#SBATCH --job-name=conda-job
#SBATCH --output=logs/conda_%j.out
#SBATCH --error=logs/conda_%j.err

#SBATCH --cpus-per-task=4
#SBATCH --mem=8G
#SBATCH --time=01:00:00

module purge
module load miniconda/VERSION

source "$(conda info --base)/etc/profile.d/conda.sh"

conda activate "$HOME/conda-envs/myproject"

echo "Conda environment: $CONDA_PREFIX"
echo "Python: $(which python)"
python --version

python analysis.py
```

!!! important
    Load Miniconda and activate the environment **inside the job script**.

[Conda / Miniconda :material-arrow-right:](../software/languages/conda.md){ .md-button }


## GPU Job

GPU jobs require the appropriate GPU partition and QOS for your account.

```bash
#!/bin/bash

#SBATCH --job-name=gpu-job
#SBATCH --output=logs/gpu_%j.out
#SBATCH --error=logs/gpu_%j.err

#SBATCH --partition=GPU_PARTITION
#SBATCH --qos=GPU_QOS

#SBATCH --gres=gpu:1
#SBATCH --cpus-per-task=4
#SBATCH --mem=16G
#SBATCH --time=01:00:00

echo "======================================"
echo "Job ID : $SLURM_JOB_ID"
echo "Node   : $(hostname)"
echo "======================================"

nvidia-smi

echo "CUDA_VISIBLE_DEVICES=$CUDA_VISIBLE_DEVICES"

./gpu_application
```

Replace:

```text
GPU_PARTITION
GPU_QOS
```

with GPU resources available to your account.

[Requesting & Using GPUs :material-arrow-right:](../gpus/using-gpus.md){ .md-button }


## AI / Machine Learning Job

Example using a Conda AI environment:

```bash
#!/bin/bash

#SBATCH --job-name=ai-training
#SBATCH --output=logs/ai_%j.out
#SBATCH --error=logs/ai_%j.err

#SBATCH --partition=GPU_PARTITION
#SBATCH --qos=GPU_QOS

#SBATCH --gres=gpu:1
#SBATCH --cpus-per-task=8
#SBATCH --mem=32G
#SBATCH --time=04:00:00

module purge
module load miniconda/VERSION

source "$(conda info --base)/etc/profile.d/conda.sh"

conda activate "$HOME/conda-envs/my-ai-env"

echo "Node: $(hostname)"

nvidia-smi

python train.py
```

For PyTorch, verify GPU access with:

```bash
python -c "import torch; print(torch.cuda.is_available())"
```

For TensorFlow:

```bash
python -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"
```

[AI & Machine Learning :material-arrow-right:](../gpus/ai-ml.md){ .md-button }


## Apptainer Job

Use this for containerized applications.

```bash
#!/bin/bash

#SBATCH --job-name=apptainer-job
#SBATCH --output=logs/apptainer_%j.out
#SBATCH --error=logs/apptainer_%j.err

#SBATCH --cpus-per-task=4
#SBATCH --mem=8G
#SBATCH --time=01:00:00

module purge
module load apptainer/VERSION

apptainer exec \
  container.sif \
  python analysis.py
```

For an NVIDIA GPU container:

```bash
apptainer exec \
  --nv \
  container.sif \
  python gpu_program.py
```

!!! important
    `--nv` exposes the GPU to the container.

    It does not allocate a GPU. The Slurm job must still request one with:

    ```bash
    #SBATCH --gres=gpu:1
    ```

[Apptainer :material-arrow-right:](../software/containers/apptainer.md){ .md-button }


## Request a Specific Account

If your workflow requires a particular Slurm account, add:

```bash
#SBATCH --account=ACCOUNT_NAME
```

For example:

```bash
#SBATCH --account=acc_example
```

Use the account associated with your research group or allocation.


## Monitor Your Job

Check job status:

```bash
squeue -u "$USER"
```

Check a completed or running job:

```bash
sacct -j JOBID
```

Check actual resource usage:

```bash
/home/share/bin/jobusage JOBID
```

For live monitoring:

```bash
/home/share/bin/jobusage -w JOBID
```

For GPU jobs:

```bash
nvidia-smi
```

You can also use the **HPC Usage** tool in Roary Open OnDemand:

```text
https://hpclogin.fiu.edu
```

A useful workflow is:

```text
Submit
  ↓
sbatch job.sh
  ↓
squeue -u $USER
  ↓
jobusage JOBID
  ↓
Adjust future resource requests
```


## Common Slurm Options

| Resource | Slurm option |
|---|---|
| Job name | `--job-name=NAME` |
| CPUs | `--cpus-per-task=N` |
| MPI ranks | `--ntasks=N` |
| Nodes | `--nodes=N` |
| Memory | `--mem=SIZE` |
| Runtime | `--time=HH:MM:SS` |
| GPU | `--gres=gpu:N` |
| Account | `--account=ACCOUNT` |
| Partition | `--partition=PARTITION` |
| QOS | `--qos=QOS` |
| Output log | `--output=FILE` |
| Error log | `--error=FILE` |


## Choosing the Right Example

```text
Normal application
      ↓
Basic CPU Job


Multithreaded application
      ↓
OpenMP Job


Multi-process / multi-node application
      ↓
MPI Job


Python / scientific environment
      ↓
Conda Job


GPU application
      ↓
GPU Job


PyTorch / TensorFlow / JAX
      ↓
AI / Machine Learning Job


Containerized application
      ↓
Apptainer Job
```


## Best Practices

- Request only the resources your application needs.
- Create separate output and error logs.
- Load required modules inside the job script.
- Use explicit software module versions when possible.
- Activate Conda environments inside the job script.
- Use `srun` for MPI jobs.
- Request GPUs only for GPU-enabled software.
- Use `/home/share/bin/jobusage JOBID` to review actual usage.
- Use `/home/share/bin/jobusage -w JOBID` for live monitoring.
- Use Open OnDemand **HPC Usage** for a graphical resource view.
- Adjust future Slurm requests based on actual resource usage.


## Related Guides

[Running Jobs](../running-jobs/index.md){ .md-button .md-button--primary }

[OpenMPI](../software/parallel/openmpi.md){ .md-button }

[OpenMP](../software/parallel/openmp.md){ .md-button }

[Conda / Miniconda](../software/languages/conda.md){ .md-button }

[GPU Computing](../gpus/index.md){ .md-button }

[Apptainer](../software/containers/apptainer.md){ .md-button }
