# Troubleshooting

<div class="troubleshooting-hero">

  <div class="troubleshooting-hero-content">

    <span class="roary-page-eyebrow">
      TROUBLESHOOTING
    </span>

    <h2>Common Roary Problems</h2>

    <p>
      Use this page as a quick reference for common job, software,
      GPU, storage, and Open OnDemand issues.
    </p>

    <p>
      Before contacting HPC support, check the commands and information
      below. Job IDs and complete error messages are especially useful.
    </p>

  </div>

  <div class="troubleshooting-hero-badge">
    <span>ROARY</span>
    <strong>HELP</strong>
    <small>SUPPORT</small>
  </div>

</div>


## Start Here

For most problems, first collect:

```bash
hostname
whoami
module list
```

Check your jobs:

```bash
squeue -u "$USER"
```

For a specific job:

```bash
scontrol show job JOBID
```

For completed jobs:

```bash
sacct -j JOBID
```

Check actual job resource usage:

```bash
/home/share/bin/jobusage JOBID
```

For live monitoring:

```bash
/home/share/bin/jobusage -w JOBID
```


## Job Is Pending

Check:

```bash
squeue -u "$USER"
```

To see why the job is waiting:

```bash
squeue -j JOBID -o "%.18i %.9P %.20j %.8u %.2t %.10M %.30R"
```

The final column shows the pending reason.

Common reasons include:

```text
Resources
Priority
QOS limits
Account limits
Partition limits
GPU availability
```

You can also check:

```bash
scontrol show job JOBID
```

!!! note
    A pending job is not necessarily an error.

    It may simply be waiting for requested resources to become available.


## Job Failed

Check the Slurm output and error files:

```bash
cat slurm-JOBID.out
```

or the files defined with:

```bash
#SBATCH --output=...
#SBATCH --error=...
```

Then check:

```bash
sacct -j JOBID \
  --format=JobID,JobName,State,ExitCode,Elapsed,MaxRSS
```

Common states include:

```text
FAILED
CANCELLED
TIMEOUT
OUT_OF_MEMORY
NODE_FAIL
```


## Job Ran Out of Memory

You may see:

```text
OUT_OF_MEMORY
```

or messages similar to:

```text
oom-kill
Killed
Out of memory
```

Check resource usage:

```bash
/home/share/bin/jobusage JOBID
```

Then increase memory only if the workload actually requires it.

For example:

```bash
#SBATCH --mem=32G
```

or:

```bash
#SBATCH --mem=64G
```

Avoid requesting very large amounts of memory without checking actual usage first.


## Job Reached the Time Limit

If the job state is:

```text
TIMEOUT
```

increase:

```bash
#SBATCH --time=HH:MM:SS
```

For example:

```bash
#SBATCH --time=08:00:00
```

Check the partition maximum time before requesting a longer runtime.


## Wrong Account or QOS

If Slurm reports an account or QOS error, check your associations:

```bash
sacctmgr show assoc user="$USER"
```

Check the job:

```bash
scontrol show job JOBID
```

Make sure the job script uses an account and QOS available to you:

```bash
#SBATCH --account=ACCOUNT_NAME
#SBATCH --qos=QOS_NAME
```


## Module Not Found

If:

```bash
module load SOFTWARE
```

fails, first search:

```bash
module avail SOFTWARE
```

or:

```bash
module avail
```

Use the exact available module name and version.

Example:

```bash
module load gcc/VERSION
```

Check loaded modules:

```bash
module list
```

If software behaves unexpectedly, start clean:

```bash
module purge
```

then load only the modules you need.

[Environment Modules :material-arrow-right:](../software/modules/index.md){ .md-button }


## Command Not Found

Check:

```bash
which PROGRAM
```

If nothing is returned, check whether a module is required:

```bash
module avail PROGRAM
```

For software installed in your account, check your `PATH`:

```bash
echo "$PATH"
```

Example:

```bash
export PATH="$HOME/software/mysoftware/bin:$PATH"
```


## Conda Environment Will Not Activate

Load Miniconda:

```bash
module avail miniconda
module load miniconda/VERSION
```

Initialize Conda for the current shell:

```bash
source "$(conda info --base)/etc/profile.d/conda.sh"
```

Then activate:

```bash
conda activate "$HOME/conda-envs/myproject"
```

Check:

```bash
which python
```

and:

```bash
echo "$CONDA_PREFIX"
```

[Conda / Miniconda :material-arrow-right:](../software/languages/conda.md){ .md-button }


## Conda Package Installation Fails

Check the active environment:

```bash
conda info --envs
```

Then confirm you are not trying to modify the centrally provided base
environment.

Install packages inside your own environment.

For example:

```bash
conda activate "$HOME/conda-envs/myproject"
```

Then install the required package.


## GPU Is Not Visible

The job must request a GPU.

Example:

```bash
#SBATCH --gres=gpu:1
```

Inside the GPU job, check:

```bash
nvidia-smi
```

and:

```bash
echo "$CUDA_VISIBLE_DEVICES"
```

If `nvidia-smi` works but the application cannot detect the GPU, the
application or Python environment may not have GPU support enabled.

[GPU Computing :material-arrow-right:](../gpus/index.md){ .md-button }


## CUDA Out of Memory

You may see:

```text
CUDA out of memory
```

Check GPU memory:

```bash
nvidia-smi
```

Possible solutions include:

- Reduce batch size
- Reduce model size
- Reduce input size
- Reduce data loaded at once
- Use a GPU with more memory when available

Remember:

```bash
#SBATCH --mem=64G
```

requests system RAM.

It does not increase GPU memory.


## GPU Utilization Is Low

Check:

```bash
nvidia-smi
```

and:

```bash
/home/share/bin/jobusage JOBID
```

Possible causes include:

- Slow data loading
- Too few CPU workers
- Small batch size
- CPU preprocessing bottleneck
- Application not fully using GPU acceleration

Use both CPU and GPU usage information before changing resource requests.


## Check Job Resource Usage

Roary provides:

```bash
/home/share/bin/jobusage JOBID
```

For example:

```bash
/home/share/bin/jobusage 123456
```

For a live view:

```bash
/home/share/bin/jobusage -w JOBID
```

This is useful for checking:

```text
CPU usage
Memory usage
GPU utilization
GPU memory usage
Requested vs. used resources
```

You can also use the **HPC Usage** tool available through Open OnDemand:

```text
https://hpclogin.fiu.edu
```


## Storage or Quota Problems

Check filesystem usage:

```bash
df -h
```

Check the size of a directory:

```bash
du -sh DIRECTORY
```

For your current directory:

```bash
du -sh .
```

Large temporary data should normally use scratch storage.

Important research data should remain in the appropriate persistent storage
location.

[Storage & Data :material-arrow-right:](../storage/index.md){ .md-button }


## Permission Denied

Check ownership and permissions:

```bash
ls -ld DIRECTORY
```

For a file:

```bash
ls -l FILE
```

Also check the parent directories:

```bash
namei -l /path/to/file
```

Do not use:

```bash
sudo
```

Users should not modify system-owned directories.


## Open OnDemand Session Is Queued

Open OnDemand applications are submitted as Slurm jobs.

Check:

```bash
squeue -u "$USER"
```

If the application is waiting, check the pending reason:

```bash
squeue -j JOBID -o "%.18i %.9P %.2t %.30R"
```

Common causes include:

```text
Resources
QOS limits
Account limits
GPU availability
```


## Open OnDemand Application Fails

Check the session logs shown in Open OnDemand.

Also check:

```bash
sacct -j JOBID
```

and:

```bash
scontrol show job JOBID
```

For Jupyter or other Python applications, also verify the Conda environment
and kernel.

[Open OnDemand :material-arrow-right:](../open-ondemand/index.md){ .md-button }


## Jupyter Kernel Missing

List available kernels:

```bash
jupyter kernelspec list
```

Activate your environment:

```bash
conda activate "$HOME/conda-envs/myproject"
```

Register it:

```bash
python -m ipykernel install \
  --user \
  --name myproject \
  --display-name "Python (myproject)"
```


## MPI Job Fails

Check the loaded MPI environment:

```bash
module list
```

Verify the MPI commands:

```bash
which mpicc
which mpirun
```

The application should normally run using the same compatible MPI environment
that was used when it was compiled.

For Slurm jobs, use the documented Roary MPI workflow.

[OpenMPI :material-arrow-right:](../software/parallel/openmpi.md){ .md-button }


## Application Works on Login but Fails in Slurm

The batch job may not have the same environment as your interactive shell.

Load everything required inside the job script.

Example:

```bash
module purge
module load SOFTWARE/VERSION
```

For Conda:

```bash
module load miniconda/VERSION
source "$(conda info --base)/etc/profile.d/conda.sh"
conda activate "$HOME/conda-envs/myproject"
```

Do not depend on software that was manually loaded before running:

```bash
sbatch job.sh
```


## Useful Diagnostic Commands

```bash
hostname
```

```bash
whoami
```

```bash
module list
```

```bash
squeue -u "$USER"
```

```bash
scontrol show job JOBID
```

```bash
sacct -j JOBID
```

```bash
/home/share/bin/jobusage JOBID
```

```bash
df -h
```

```bash
nvidia-smi
```


## Before Contacting HPC Support

Please provide:

- Your username
- Job ID
- Software name and version
- Complete job script
- Output file
- Error file
- Exact error message
- Approximate time the problem occurred

For software problems, also provide:

```bash
module list
```

For job problems:

```bash
scontrol show job JOBID
```

and:

```bash
sacct -j JOBID
```

For resource problems:

```bash
/home/share/bin/jobusage JOBID
```

For GPU problems:

```bash
nvidia-smi
```

This information helps the HPC team diagnose the problem faster.


## Need Help?

For assistance:

[**hpcadmin@fiu.edu**](mailto:hpcadmin@fiu.edu)


## Related Guides

[Running Jobs](../running-jobs/index.md){ .md-button .md-button--primary }

[Environment Modules](../software/modules/index.md){ .md-button }

[Conda / Miniconda](../software/languages/conda.md){ .md-button }

[GPU Computing](../gpus/index.md){ .md-button }

[Open OnDemand](../open-ondemand/index.md){ .md-button }

[Storage & Data](../storage/index.md){ .md-button }
