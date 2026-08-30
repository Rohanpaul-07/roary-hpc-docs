# Best Practices

<div class="best-practices-hero">

  <div class="best-practices-hero-content">

    <span class="roary-page-eyebrow">
      BEST PRACTICES
    </span>

    <h2>Using Roary Efficiently</h2>

    <p>
      Following a few simple practices helps jobs run efficiently,
      improves reproducibility, and keeps shared Roary resources
      available for everyone.
    </p>

  </div>

  <div class="best-practices-hero-badge">
    <span>ROARY</span>
    <strong>BEST</strong>
    <small>PRACTICES</small>
  </div>

</div>


## Use Slurm for Computation

Login nodes are intended for light tasks such as:

- Editing files
- Managing software environments
- Compiling small programs
- Submitting jobs
- Checking job status

Do not run long or resource-intensive applications directly on login nodes.

Submit computational workloads with:

```bash
sbatch job.sh
```

[Running Jobs :material-arrow-right:](../running-jobs/index.md){ .md-button }


## Request Only What You Need

A typical job may request:

```bash
#SBATCH --cpus-per-task=4
#SBATCH --mem=16G
#SBATCH --time=02:00:00
```

Avoid requesting significantly more CPUs, memory, GPUs, or runtime than your
application needs.

Larger requests can increase queue time and reduce cluster efficiency.


## Check Your Resource Usage

Roary provides different tools depending on what you want to review.

### Overall HPC Usage

For overall account, storage, allocation, and QOS information, use:

```bash
/home/share/bin/hpcusage
```

You can also view this information graphically through:

```text
Open OnDemand
      ↓
HPC Usage
```

Open OnDemand:

```text
https://hpclogin.fiu.edu
```


### Individual Job Usage

For a specific Slurm job, use:

```bash
/home/share/bin/jobusage JOBID
```

For example:

```bash
/home/share/bin/jobusage 123456
```

For live monitoring:

```bash
/home/share/bin/jobusage -w JOBID
```

Find your active job IDs with:

```bash
squeue -u "$USER"
```


### Which Tool Should I Use?

```text
Overall account / storage / allocation
              ↓
   /home/share/bin/hpcusage

              or

   Open OnDemand → HPC Usage


Individual Slurm job
              ↓
/home/share/bin/jobusage JOBID


Live individual job
              ↓
/home/share/bin/jobusage -w JOBID
```

!!! tip
    Use `hpcusage` for overall Roary account, storage, and allocation
    information.

    Use `jobusage` when checking the actual resource utilization of an
    individual Slurm job.

Use actual resource usage to improve future Slurm requests.


## Use Environment Modules

Check whether software is already available before installing another copy:

```bash
module avail SOFTWARE
```

Load the required version:

```bash
module load SOFTWARE/VERSION
```

Check your environment:

```bash
module list
```

For reproducible jobs, start with:

```bash
module purge
```

and explicitly load the modules required by the job.

[Environment Modules :material-arrow-right:](../software/modules/index.md){ .md-button }


## Use Explicit Software Versions

When possible, load a specific software version:

```bash
module load gcc/VERSION
```

rather than depending on whichever version is currently the default.

This is especially important for:

```text
GCC
OpenMPI
CUDA
Miniconda
Apptainer
Scientific applications
```

Recording software versions makes workflows easier to reproduce.


## Keep Software Environments Organized

For personal Conda environments, use a consistent location such as:

```text
$HOME/conda-envs/
```

For manually installed software:

```text
$HOME/software/
```

For example:

```text
$HOME/
├── conda-envs/
├── software/
└── projects/
```

Separate environments for different projects can help prevent dependency
conflicts.

[Conda / Miniconda :material-arrow-right:](../software/languages/conda.md){ .md-button }


## Activate Environments Inside Job Scripts

Do not assume your interactive shell environment will automatically be
available inside a Slurm job.

For Conda:

```bash
module purge
module load miniconda/VERSION

source "$(conda info --base)/etc/profile.d/conda.sh"

conda activate "$HOME/conda-envs/myproject"

python analysis.py
```

The job script should contain everything required to reproduce the software
environment.


## Use the Correct Storage

Use persistent storage for important files such as:

```text
Source code
Job scripts
Research results
Software environments
Container images
Important project data
```

Use scratch for temporary computational data and intermediate files.

!!! warning
    Scratch is temporary storage.

    Files that have not been used for 30 days may be automatically purged
    according to IRCC storage policy.

    Do not keep the only copy of important research data in scratch.

[Storage & Data :material-arrow-right:](../storage/index.md){ .md-button }


## Keep Job Scripts and Logs

Keep the Slurm scripts used for important computational results.

A useful project structure is:

```text
project/
├── scripts/
├── jobs/
├── logs/
├── input/
└── results/
```

Use separate Slurm output and error logs:

```bash
#SBATCH --output=logs/job_%j.out
#SBATCH --error=logs/job_%j.err
```

The `%j` value is replaced by the Slurm job ID.


## Use Descriptive Job Names

Instead of:

```bash
#SBATCH --job-name=test
```

use something meaningful:

```bash
#SBATCH --job-name=model-training
```

or:

```bash
#SBATCH --job-name=genome-align
```


## CPU Jobs

For a single-process application:

```bash
#SBATCH --cpus-per-task=1
```

For multithreaded software:

```bash
#SBATCH --cpus-per-task=8
```

If using OpenMP:

```bash
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
```

Do not request many CPU cores for software that only uses one.


## MPI Jobs

For MPI applications, Slurm tasks normally represent MPI ranks.

Example:

```bash
#SBATCH --nodes=2
#SBATCH --ntasks=16
#SBATCH --ntasks-per-node=8
```

Run using the documented MPI environment:

```bash
srun ./my_mpi_program
```

Use a compatible MPI environment during both compilation and runtime.

[OpenMPI :material-arrow-right:](../software/parallel/openmpi.md){ .md-button }


## GPU Jobs

Request a GPU only when the application supports GPU acceleration.

Example:

```bash
#SBATCH --gres=gpu:1
```

Inside the job:

```bash
nvidia-smi
```

Check the GPU exposed by Slurm:

```bash
echo "$CUDA_VISIBLE_DEVICES"
```

For complete job resource usage:

```bash
/home/share/bin/jobusage JOBID
```

!!! important
    Requesting a GPU does not automatically make a CPU application faster.

    The application itself must support GPU acceleration.

[GPU Computing :material-arrow-right:](../gpus/index.md){ .md-button }


## Monitor GPU Workloads

For GPU utilization and GPU memory:

```bash
nvidia-smi
```

For continuous monitoring:

```bash
watch -n 2 nvidia-smi
```

For the complete Slurm job:

```bash
/home/share/bin/jobusage JOBID
```

Use both tools together:

```text
nvidia-smi
     ↓
GPU utilization and GPU memory


jobusage JOBID
     ↓
CPU, memory, GPU, and job utilization
```


## Use Apptainer for Containers

Roary uses Apptainer for HPC container workflows.

Run a container with:

```bash
apptainer exec container.sif COMMAND
```

For NVIDIA GPU containers:

```bash
apptainer exec --nv container.sif COMMAND
```

[Apptainer :material-arrow-right:](../software/containers/apptainer.md){ .md-button }


## Do Not Use sudo

Users should not use:

```bash
sudo
```

for software installation or system changes.

Install personal software under locations you own, such as:

```text
$HOME/software/
```

For shared or complex software, submit a software request.

[Request New Software :material-arrow-right:](../software/admin/request-software.md){ .md-button }


## Clean Up Unused Files

Periodically remove unnecessary:

- Temporary files
- Old job output
- Unused Conda environments
- Old software builds
- Unneeded container caches
- Duplicate datasets

Check directory size:

```bash
du -sh DIRECTORY
```

Check Apptainer cache:

```bash
apptainer cache list
```

Clean unused cache when appropriate:

```bash
apptainer cache clean
```


## End Open OnDemand Sessions

When using Jupyter, RStudio, Code Server, or another interactive application:

1. Save your work.
2. Close the application.
3. Return to the Open OnDemand session page.
4. Stop or delete the session.

Closing only the browser tab may not immediately terminate the underlying
Slurm job.

[Open OnDemand :material-arrow-right:](../open-ondemand/index.md){ .md-button }


## Test Before Running Large Jobs

Test a small workload first:

```text
Small input
     ↓
Short test job
     ↓
Check output
     ↓
Check jobusage
     ↓
Adjust resources
     ↓
Submit full workload
```

This can prevent large jobs from failing because of incorrect paths,
software environments, memory requests, or runtime estimates.


## Recommended Workflow

```text
Check software
      ↓
module avail

Prepare environment
      ↓
Modules / Conda / Apptainer

Create Slurm job
      ↓
Request appropriate resources

Test small workload
      ↓
Confirm application works

Submit
      ↓
sbatch job.sh

Monitor
      ↓
squeue / jobusage / nvidia-smi

Review usage
      ↓
Improve future resource requests
```


## Quick Checklist

Before submitting a job:

- Check the account, partition, and QOS.
- Request appropriate CPUs and memory.
- Request a reasonable runtime.
- Request GPUs only when necessary.
- Load required modules.
- Activate required environments.
- Verify input paths.
- Create output and log directories.

After the job finishes:

- Check the output.
- Check the error log.
- Check the job state.
- Review `/home/share/bin/jobusage JOBID`.
- Keep important results.
- Remove unnecessary temporary data.


## Need Help?

For assistance:

[**hpcadmin@fiu.edu**](mailto:hpcadmin@fiu.edu)


## Related Guides

[Running Jobs](../running-jobs/index.md){ .md-button .md-button--primary }

[Job Examples](../examples/index.md){ .md-button }

[Troubleshooting](../troubleshooting/index.md){ .md-button }

[Storage & Data](../storage/index.md){ .md-button }

[GPU Computing](../gpus/index.md){ .md-button }

[Open OnDemand](../open-ondemand/index.md){ .md-button }
