# Open OnDemand

<div class="ood-hero">

  <div class="ood-hero-content">

    <span class="roary-page-eyebrow">
      OPEN ONDEMAND
    </span>

    <h2>Access Roary from Your Web Browser</h2>

    <p>
      Open OnDemand provides browser-based access to Roary without requiring
      users to work entirely from an SSH terminal.
    </p>

    <p>
      You can manage files, open a terminal, launch interactive applications,
      and review HPC usage directly from the Roary Open OnDemand portal.
    </p>

  </div>

  <div class="ood-hero-badge">
    <span>ROARY</span>
    <strong>OOD</strong>
    <small>WEB PORTAL</small>
  </div>

</div>


## Access Open OnDemand

Open:

```text
https://hpclogin.fiu.edu
```

Sign in using your FIU credentials.

The Open OnDemand dashboard provides access to Roary tools and interactive
applications.


## What Can I Do in Open OnDemand?

From the dashboard, users can access features such as:

- File management
- Shell access
- Interactive applications
- Jupyter
- RStudio
- Code Server / VS Code
- HPC Usage
- Active interactive sessions

Available applications may vary depending on your account and access.


## Files

Open OnDemand provides a browser-based file manager.

Use it to:

- Browse directories
- Upload files
- Download files
- Rename files
- Create directories
- Delete files

For large research data transfers, use the recommended Roary data-transfer
methods rather than uploading large datasets through the browser.

[Storage & Data :material-arrow-right:](../storage/index.md){ .md-button }


## Shell Access

Open OnDemand provides a terminal directly in your browser.

The shell provides command-line access similar to:

```bash
ssh USERNAME@hpclogin.fiu.edu
```

You can use normal Roary commands such as:

```bash
module avail
```

```bash
squeue -u "$USER"
```

```bash
sbatch job.sh
```

!!! warning
    The Open OnDemand shell is still a login-node shell.

    Do not run CPU-intensive, memory-intensive, long-running, or GPU
    workloads directly in the shell.


## Interactive Applications

Open OnDemand can launch interactive applications on Roary compute resources.

Examples may include:

```text
Jupyter
RStudio
Code Server / VS Code
Other supported interactive applications
```

When launching an application, you may be asked to select resources such as:

- Account
- Partition
- QOS
- Number of CPU cores
- Memory
- Runtime
- GPU resources, when applicable

Open OnDemand submits the request to Slurm:

```text
Open OnDemand
      ↓
Slurm job submitted
      ↓
Wait for resources
      ↓
Compute node assigned
      ↓
Application starts
```

!!! important
    Open OnDemand interactive applications still use Slurm.

    Request only the resources required by your workload.


## Active Interactive Sessions

After requesting an interactive application, Open OnDemand displays the
session status.

A session may show:

```text
Queued
Starting
Running
Completed
```

If the requested resources are not immediately available, the session may
remain queued.

When the application is ready, use the **Launch** or **Connect** button shown
for the session.


## Jupyter

Jupyter can be launched through Open OnDemand for interactive Python and
notebook workflows.

If you created your own Conda environment, you can register it as a Jupyter
kernel.

Load Miniconda:

```bash
module load miniconda/VERSION
```

Prepare Conda:

```bash
source "$(conda info --base)/etc/profile.d/conda.sh"
```

Activate your environment:

```bash
conda activate "$HOME/conda-envs/myproject"
```

Install `ipykernel` if needed:

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

The kernel can then be selected from Jupyter.

[Conda / Miniconda :material-arrow-right:](../software/languages/conda.md){ .md-button }


## GPU Interactive Applications

If an Open OnDemand application supports GPUs, request GPU resources when
creating the session.

GPU access depends on the resources available to your Roary account.

Inside the GPU session, verify GPU access with:

```bash
nvidia-smi
```

Check the GPUs exposed to the session:

```bash
echo "$CUDA_VISIBLE_DEVICES"
```

[GPU Computing :material-arrow-right:](../gpus/index.md){ .md-button }


## HPC Usage

The Open OnDemand dashboard includes the **HPC Usage** tool.

Use it to review overall Roary information such as:

- Storage usage and quotas
- Slurm account information
- QOS information
- Allocation information
- Overall resource usage

Open:

```text
https://hpclogin.fiu.edu
```

and select:

```text
HPC Usage
```

from the dashboard.


### Command-Line HPC Usage

The same overall usage information can also be viewed from the terminal.

Use:

```bash
/home/share/bin/hpcusage
```

Use `hpcusage` when you want to review your overall:

```text
Account
Storage
Allocation
QOS
HPC usage
```


## Individual Job Usage

For resource usage of a specific Slurm job, use:

```bash
/home/share/bin/jobusage JOBID
```

For example:

```bash
/home/share/bin/jobusage 123456
```

For live monitoring while the job is running:

```bash
/home/share/bin/jobusage -w JOBID
```

Find your active jobs with:

```bash
squeue -u "$USER"
```


## Which Usage Tool Should I Use?

```text
Overall account / storage / allocation
              ↓
Open OnDemand → HPC Usage

              or

/home/share/bin/hpcusage


Individual Slurm job
              ↓
/home/share/bin/jobusage JOBID


Live individual job monitoring
              ↓
/home/share/bin/jobusage -w JOBID
```

!!! tip
    Use **HPC Usage** or `hpcusage` for overall Roary account and
    allocation information.

    Use `jobusage` when checking the resource utilization of a specific
    Slurm job.


## Files Created by Interactive Applications

Interactive applications may create files in your home or project
directories, including:

- Notebook files
- Application configuration
- Logs
- Output files
- Conda environments
- Jupyter kernels

Keep important files in persistent storage.

Use scratch for temporary computational data when appropriate.


## Ending an Interactive Session

When finished with an interactive application:

1. Save your work.
2. Close the application.
3. Return to the Open OnDemand session page.
4. Stop or delete the session if it is still running.

!!! important
    Closing only the browser tab may not immediately terminate the underlying
    Slurm job.

    End the interactive session when you are finished so allocated resources
    are released.


## Common Problems

### Interactive Session Stays Queued

Open OnDemand applications are submitted through Slurm.

Check your jobs:

```bash
squeue -u "$USER"
```

To see why a job is waiting:

```bash
squeue -j JOBID -o "%.18i %.9P %.2t %.30R"
```

A job may wait because of:

- Resource availability
- Partition availability
- QOS limits
- Account limits
- GPU availability


### Application Fails to Start

Check the interactive session output or log files shown by Open OnDemand.

Also check:

```bash
squeue -u "$USER"
```

For a completed job:

```bash
sacct -j JOBID
```


### Jupyter Kernel Is Missing

Check available kernels:

```bash
jupyter kernelspec list
```

Activate the Conda environment:

```bash
conda activate "$HOME/conda-envs/myproject"
```

Register it again if needed:

```bash
python -m ipykernel install \
  --user \
  --name myproject \
  --display-name "Python (myproject)"
```


### GPU Is Not Visible

Inside the GPU session:

```bash
nvidia-smi
```

and:

```bash
echo "$CUDA_VISIBLE_DEVICES"
```

Make sure the interactive session was submitted with GPU resources.


## Best Practices

- Use Open OnDemand for browser-based access to Roary.
- Use the Files application for normal file management.
- Use the Shell application for command-line access and job submission.
- Run computational work through Slurm or interactive applications.
- Request only the CPU, memory, runtime, and GPU resources you need.
- Use **HPC Usage** or `/home/share/bin/hpcusage` for overall account, storage, and allocation information.
- Use `/home/share/bin/jobusage JOBID` for individual Slurm job utilization.
- Use `/home/share/bin/jobusage -w JOBID` for live job monitoring.
- End interactive sessions when you are finished.
- Do not run heavy computational workloads directly on login nodes.
- Use appropriate storage and transfer tools for large datasets.


## Need Help?

For Open OnDemand problems, provide:

- Application being launched
- Approximate time of the problem
- Job ID, if available
- Screenshot or error message
- Interactive session output or log

Check your jobs with:

```bash
squeue -u "$USER"
```

For a specific job:

```bash
sacct -j JOBID
```

For job resource usage:

```bash
/home/share/bin/jobusage JOBID
```

For assistance:

[**hpcadmin@fiu.edu**](mailto:hpcadmin@fiu.edu)


## Related Guides

[Getting Started](../getting-started/index.md){ .md-button }

[Running Jobs](../running-jobs/index.md){ .md-button }

[Storage & Data](../storage/index.md){ .md-button }

[GPU Computing](../gpus/index.md){ .md-button }

[Conda / Miniconda](../software/languages/conda.md){ .md-button }
