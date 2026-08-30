# Policies

<div class="policies-hero">

  <div class="policies-hero-content">

    <span class="roary-page-eyebrow">
      ROARY POLICIES
    </span>

    <h2>Responsible Use of Roary</h2>

    <p>
      Roary is a shared research computing environment. Users are expected
      to use compute, storage, software, and network resources responsibly
      and without interfering with other researchers.
    </p>

    <p>
      FIU policies and applicable research data requirements also remain
      in effect when using Roary.
    </p>

  </div>

  <div class="policies-hero-badge">
    <span>ROARY</span>
    <strong>POLICY</strong>
    <small>HPC</small>
  </div>

</div>


## Account Usage

Roary accounts are assigned to individual users.

- Do not share your account with another person.
- Do not share passwords or authentication credentials.
- Use only the Slurm accounts and resources assigned to you.
- Contact the HPC team if access to another research group or allocation is required.

For assistance:

[**hpcadmin@fiu.edu**](mailto:hpcadmin@fiu.edu)


## No Personal Use

Roary resources are provided for authorized research, instructional,
and academic computing.

Personal file storage or unrelated personal use of HPC resources is not
permitted.

Examples include:

- Personal movie or video collections
- Personal music collections
- Unrelated personal backups
- Personal workloads unrelated to research or instruction

Use Roary resources only for appropriate FIU research, academic,
and instructional activities.


## Login Nodes

Login nodes are shared resources intended for light tasks such as:

- Editing files
- Managing files
- Managing software environments
- Compiling small programs
- Submitting Slurm jobs
- Checking job status

Do not run long-running or resource-intensive computation directly on
login nodes.

Submit computational workloads through Slurm:

```bash
sbatch job.sh
```

For interactive computation, request resources through Slurm or use
Open OnDemand.

[Running Jobs :material-arrow-right:](../running-jobs/index.md){ .md-button }


## Compute Resources

CPU, memory, GPU, and runtime resources must be requested through Slurm.

Example:

```bash
#SBATCH --cpus-per-task=4
#SBATCH --mem=16G
#SBATCH --time=02:00:00
```

GPU workloads must explicitly request GPU resources:

```bash
#SBATCH --gres=gpu:1
```

Request only the resources your workload reasonably needs.

Unnecessarily large requests can increase queue time and reduce resource
availability for other users.


## Monitor Resource Usage

For overall account, storage, allocation, and QOS information:

```bash
/home/share/bin/hpcusage
```

You can also use the **HPC Usage** tool in Open OnDemand:

```text
https://hpclogin.fiu.edu
```

For an individual Slurm job:

```bash
/home/share/bin/jobusage JOBID
```

For live job monitoring:

```bash
/home/share/bin/jobusage -w JOBID
```

For GPU workloads:

```bash
nvidia-smi
```

Use actual resource utilization to improve future Slurm requests.


## Storage

Use persistent storage for important files such as:

```text
Source code
Job scripts
Research results
Software environments
Important project data
```

Use scratch for temporary computational data and intermediate files.

!!! warning
    Scratch is temporary storage.

    Files that have not been used for 30 days may be automatically purged
    according to IRCC storage policy.

    Do not keep the only copy of important research data in scratch.

[Storage & Data :material-arrow-right:](../storage/index.md){ .md-button }


## Storage Quotas

Users and research groups may have storage quotas.

Check directory usage with:

```bash
du -sh DIRECTORY
```

Users should periodically remove unnecessary:

- Temporary files
- Duplicate datasets
- Old job output
- Unused software environments
- Unneeded container caches

If additional long-term storage is required, contact the HPC team.


## Data Responsibility

Users are responsible for ensuring that research data stored on Roary is
handled according to applicable FIU and research requirements.

Do not assume that every storage location has the same backup or retention
characteristics.

For important research data:

- Use the appropriate project storage location.
- Maintain appropriate copies when required.
- Follow applicable FIU data-handling requirements.
- Do not use scratch as the only copy.


## Software

Before installing software yourself, check whether it is already available:

```bash
module avail SOFTWARE_NAME
```

Use centrally provided modules when available.

Personal software may be installed in user-owned locations such as:

```text
$HOME/software/
```

Do not attempt system-level installations using:

```bash
sudo
```

For complex or shared software, submit a software request.

[Installing Your Own Software](../software/admin/installing-software.md){ .md-button }

[Request New Software](../software/admin/request-software.md){ .md-button }


## Conda Environments

Create Conda environments inside storage owned by your account.

A recommended location is:

```text
$HOME/conda-envs/
```

Do not modify the centrally installed Conda base environment.

Use separate environments for different projects when practical.

[Conda / Miniconda :material-arrow-right:](../software/languages/conda.md){ .md-button }


## Containers

Roary uses Apptainer for HPC container workloads.

Containers follow the same Slurm and storage policies as other applications.

Running a container does not bypass:

```text
CPU limits
Memory limits
GPU allocation
Storage permissions
Slurm policies
```

GPU containers must still request GPU resources through Slurm.

[Apptainer :material-arrow-right:](../software/containers/apptainer.md){ .md-button }


## GPU Usage

Request GPU resources only for applications that support GPU acceleration.

Example:

```bash
#SBATCH --gres=gpu:1
```

Inside a GPU job:

```bash
nvidia-smi
```

Check the GPU exposed by Slurm:

```bash
echo "$CUDA_VISIBLE_DEVICES"
```

Do not request multiple GPUs unless the application is configured to use them.

[GPU Computing :material-arrow-right:](../gpus/index.md){ .md-button }


## Open OnDemand

Open OnDemand interactive applications consume Slurm resources.

Examples include:

```text
Jupyter
RStudio
Code Server
GPU interactive sessions
```

Stop interactive sessions when they are no longer needed.

Closing only the browser tab may not immediately terminate the underlying
Slurm job.

Return to the Open OnDemand session page and stop or delete the session when
finished.

[Open OnDemand :material-arrow-right:](../open-ondemand/index.md){ .md-button }


## Network and External Access

Roary should be used only for legitimate research, educational, and
computational activities.

Do not use HPC resources to:

- Circumvent security or network controls
- Run unauthorized network services
- Attempt unauthorized access to systems or data
- Interfere with other systems or users

If a research workflow requires special network access, contact the HPC team.


## Shared Resources

Roary is a multi-user system.

Avoid activities that unnecessarily affect other researchers, including:

- Heavy computation on login nodes
- Excessive CPU or memory requests
- Unnecessary GPU reservations
- Excessive temporary storage use
- Large uncontrolled builds on login nodes
- Leaving interactive sessions running when no longer needed


## Security

Protect your account and research data.

Do not place credentials such as:

```text
Passwords
API keys
Access tokens
Private keys
```

directly inside job scripts or source code when avoidable.

Review permissions when necessary:

```bash
ls -l
```

and:

```bash
ls -ld DIRECTORY
```


## Report Problems

Contact the HPC team if you notice:

- Unexpected account activity
- Access to resources you should not have
- Suspicious files or processes
- Security concerns
- Repeated system failures
- Problems affecting multiple users

For assistance:

[**hpcadmin@fiu.edu**](mailto:hpcadmin@fiu.edu)


## Quick Policy Summary

```text
Use your own account
        ↓
No personal use
        ↓
Use login nodes for light work only
        ↓
Run computation through Slurm
        ↓
Request only necessary resources
        ↓
Monitor resource usage
        ↓
Use storage appropriately
        ↓
Protect research data and credentials
        ↓
Stop unused interactive sessions
```


## Related Guides

[Best Practices](../best-practices/index.md){ .md-button .md-button--primary }

[Running Jobs](../running-jobs/index.md){ .md-button }

[Storage & Data](../storage/index.md){ .md-button }

[GPU Computing](../gpus/index.md){ .md-button }

[Open OnDemand](../open-ondemand/index.md){ .md-button }

[Troubleshooting](../troubleshooting/index.md){ .md-button }
