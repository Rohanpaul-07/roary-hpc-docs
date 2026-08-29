# Running Jobs

<div class="jobs-hero">

  <div class="jobs-hero-content">

    <span class="roary-page-eyebrow">
      SLURM WORKLOAD MANAGEMENT
    </span>

    <h2>
      Request resources. Submit work. Let Roary schedule it.
    </h2>

    <p>
      Roary uses the Slurm workload manager to schedule computational jobs.
      Instead of running large applications directly on a login node, you
      describe the resources your workload needs and submit the job to Slurm.
    </p>

  </div>


  <div class="jobs-hero-badge">

    <span>ROARY</span>

    <strong>SLURM</strong>

    <small>SCHEDULER</small>

  </div>

</div>


## The Basic Job Workflow

<div class="jobs-workflow">

  <div class="jobs-workflow-step">

    <span>01</span>

    <div class="jobs-workflow-symbol">
      SCRIPT
    </div>

    <strong>
      Prepare
    </strong>

    <p>
      Create your application command and Slurm job script.
    </p>

  </div>


  <div class="jobs-workflow-arrow">
    →
  </div>


  <div class="jobs-workflow-step">

    <span>02</span>

    <div class="jobs-workflow-symbol">
      SBATCH
    </div>

    <strong>
      Submit
    </strong>

    <p>
      Send the job to Slurm with <code>sbatch</code>.
    </p>

  </div>


  <div class="jobs-workflow-arrow">
    →
  </div>


  <div class="jobs-workflow-step jobs-workflow-slurm">

    <span>03</span>

    <div class="jobs-workflow-symbol">
      SLURM
    </div>

    <strong>
      Schedule
    </strong>

    <p>
      Slurm waits for resources that satisfy your request.
    </p>

  </div>


  <div class="jobs-workflow-arrow">
    →
  </div>


  <div class="jobs-workflow-step">

    <span>04</span>

    <div class="jobs-workflow-symbol">
      NODE
    </div>

    <strong>
      Run
    </strong>

    <p>
      Your workload executes on allocated compute resources.
    </p>

  </div>


  <div class="jobs-workflow-arrow">
    →
  </div>


  <div class="jobs-workflow-step">

    <span>05</span>

    <div class="jobs-workflow-symbol">
      DATA
    </div>

    <strong>
      Review
    </strong>

    <p>
      Inspect output, job status, and resource utilization.
    </p>

  </div>

</div>


## Your First Batch Job

A Slurm batch job is usually described in a shell script containing
resource requests followed by the commands you want to run.

Create a file such as:

```bash
vi myjob.slurm
```

A simple example:

```bash
#!/bin/bash

#SBATCH --job-name=my-analysis
#SBATCH --output=my-analysis_%j.out
#SBATCH --error=my-analysis_%j.err
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1
#SBATCH --mem=2G
#SBATCH --time=00:30:00

echo "Job ID: $SLURM_JOB_ID"
echo "Running on: $(hostname)"
echo "Started: $(date)"

# Replace this line with your application command
sleep 30

echo "Finished: $(date)"
```

Then submit it:

```bash
sbatch myjob.slurm
```

Slurm returns a unique job ID:

```text
Submitted batch job 123456
```


## Understanding the Resource Request

<div class="job-resource-grid">

  <div class="job-resource-card">

    <span>NAME</span>

    <strong>
      Job Name
    </strong>

    <code>
      --job-name
    </code>

    <p>
      A descriptive name shown in Slurm commands.
    </p>

  </div>


  <div class="job-resource-card">

    <span>CPU</span>

    <strong>
      CPU Cores
    </strong>

    <code>
      --cpus-per-task
    </code>

    <p>
      Number of CPU cores requested for the task.
    </p>

  </div>


  <div class="job-resource-card">

    <span>RAM</span>

    <strong>
      Memory
    </strong>

    <code>
      --mem
    </code>

    <p>
      Memory reserved for the job.
    </p>

  </div>


  <div class="job-resource-card">

    <span>TIME</span>

    <strong>
      Runtime
    </strong>

    <code>
      --time
    </code>

    <p>
      Maximum amount of time the job may run.
    </p>

  </div>


  <div class="job-resource-card">

    <span>OUT</span>

    <strong>
      Standard Output
    </strong>

    <code>
      --output
    </code>

    <p>
      File where normal program output is written.
    </p>

  </div>


  <div class="job-resource-card">

    <span>ERR</span>

    <strong>
      Standard Error
    </strong>

    <code>
      --error
    </code>

    <p>
      File where errors and diagnostic messages are written.
    </p>

  </div>

</div>

`%j` in an output filename is replaced automatically with the Slurm job ID.


## Essential Slurm Commands

<div class="slurm-command-grid">

  <div class="slurm-command-card">

    <span>SUBMIT</span>

    <strong>
      Submit a batch job
    </strong>

```bash
sbatch job.slurm
```

  </div>


  <div class="slurm-command-card">

    <span>QUEUE</span>

    <strong>
      View your active jobs
    </strong>

```bash
squeue -u $USER
```

  </div>


  <div class="slurm-command-card">

    <span>DETAILS</span>

    <strong>
      Inspect a job
    </strong>

```bash
scontrol show job JOBID
```

  </div>


  <div class="slurm-command-card">

    <span>HISTORY</span>

    <strong>
      View job accounting
    </strong>

```bash
sacct -j JOBID
```

  </div>


  <div class="slurm-command-card">

    <span>CANCEL</span>

    <strong>
      Cancel a job
    </strong>

```bash
scancel JOBID
```

  </div>


  <div class="slurm-command-card">

    <span>RESOURCES</span>

    <strong>
      View cluster availability
    </strong>

```bash
sinfo
```

  </div>

</div>


## Job States

When you run:

```bash
squeue -u $USER
```

the `ST` column shows the current state of each job.

<div class="slurm-state-grid">

  <div class="slurm-state-card">

    <span>PD</span>

    <strong>
      Pending
    </strong>

    <p>
      Waiting for resources or another scheduling condition.
    </p>

  </div>


  <div class="slurm-state-card">

    <span>R</span>

    <strong>
      Running
    </strong>

    <p>
      The workload is currently executing.
    </p>

  </div>


  <div class="slurm-state-card">

    <span>CG</span>

    <strong>
      Completing
    </strong>

    <p>
      The job is finishing and Slurm is cleaning up resources.
    </p>

  </div>


  <div class="slurm-state-card">

    <span>CD</span>

    <strong>
      Completed
    </strong>

    <p>
      The job finished successfully from Slurm's perspective.
    </p>

  </div>


  <div class="slurm-state-card">

    <span>F</span>

    <strong>
      Failed
    </strong>

    <p>
      The job terminated unsuccessfully.
    </p>

  </div>


  <div class="slurm-state-card">

    <span>TO</span>

    <strong>
      Timeout
    </strong>

    <p>
      The job reached its requested runtime limit.
    </p>

  </div>

</div>


## Why Is My Job Pending?

A pending job is not necessarily a problem.

Slurm may keep a job in the queue while it waits for the requested
resources or scheduling conditions.

Check your jobs:

```bash
squeue -u $USER
```

For more detail on a particular job:

```bash
scontrol show job JOBID
```

Common reasons may include:

<div class="pending-reason-grid">

  <div>
    <span>RESOURCES</span>
    <strong>Requested resources are currently busy</strong>
  </div>

  <div>
    <span>PRIORITY</span>
    <strong>Other eligible jobs currently have higher scheduling priority</strong>
  </div>

  <div>
    <span>QOS</span>
    <strong>A scheduling or QoS limit is affecting the job</strong>
  </div>

  <div>
    <span>DEPENDENCY</span>
    <strong>The job is waiting for another required job or condition</strong>
  </div>

</div>


## Choosing Resources

Requesting more resources does not automatically make a job faster.

<div class="request-balance-panel">

  <div class="request-balance-item">

    <span>TOO SMALL</span>

    <strong>
      Job may fail
    </strong>

    <p>
      Too little memory, runtime, or other required resources can cause
      the workload to terminate.
    </p>

  </div>


  <div class="request-balance-center">

    <span>RIGHT SIZE</span>

    <strong>
      Efficient Request
    </strong>

    <small>
      Match resources to workload
    </small>

  </div>


  <div class="request-balance-item">

    <span>TOO LARGE</span>

    <strong>
      Job may wait longer
    </strong>

    <p>
      Large CPU, memory, GPU, or runtime requests can be harder for
      Slurm to schedule.
    </p>

  </div>

</div>


<div class="job-resource-note">

  <div class="job-note-mark">
    !
  </div>

  <div>

    <span class="roary-page-eyebrow">
      BEST PRACTICE
    </span>

    <h3>
      Request what your workload actually needs
    </h3>

    <p>
      Start with a reasonable resource request, review the actual
      utilization after the job finishes, and adjust future jobs based
      on measured usage.
    </p>

  </div>

</div>


## Monitor Resource Usage

For supported jobs, Roary provides the JobUsage utility.

Run:

```bash
/home/share/bin/jobusage JOBID
```

Example:

```bash
/home/share/bin/jobusage 123456
```

JobUsage can help you inspect how effectively the workload is using
allocated resources.

Depending on the job and available metrics, this may include:

<div class="jobusage-grid">

  <div>
    <span>CPU</span>
    <strong>CPU utilization</strong>
  </div>

  <div>
    <span>RAM</span>
    <strong>Memory utilization</strong>
  </div>

  <div>
    <span>GPU</span>
    <strong>GPU utilization</strong>
  </div>

  <div>
    <span>VRAM</span>
    <strong>GPU memory usage</strong>
  </div>

</div>


## Batch Jobs vs Interactive Work

<div class="job-mode-grid">

  <div class="job-mode-card">

    <span class="job-mode-code">
      BATCH
    </span>

    <strong>
      Batch Jobs
    </strong>

    <p>
      Best for workloads that can run without continuous user interaction.
      Create a script, submit it, and allow Slurm to execute the work when
      resources become available.
    </p>

    <code>
      sbatch job.slurm
    </code>

  </div>


  <div class="job-mode-card job-mode-interactive">

    <span class="job-mode-code">
      INTERACTIVE
    </span>

    <strong>
      Interactive Computing
    </strong>

    <p>
      Useful when you need direct interaction with compute resources,
      development environments, notebooks, desktops, or graphical applications.
    </p>

    <a href="../open-ondemand/">
      Open OnDemand guide →
    </a>

  </div>

</div>


## More Advanced Job Types

As your workloads become more complex, Roary can support additional
Slurm workflows.

<div class="advanced-job-grid">

  <div class="advanced-job-card">

    <span>ARRAY</span>

    <strong>
      Job Arrays
    </strong>

    <p>
      Run many similar tasks using one Slurm submission.
    </p>

  </div>


  <div class="advanced-job-card">

    <span>MPI</span>

    <strong>
      Distributed Jobs
    </strong>

    <p>
      Run applications designed to communicate across multiple processes.
    </p>

  </div>


  <div class="advanced-job-card">

    <span>OMP</span>

    <strong>
      Multithreaded Jobs
    </strong>

    <p>
      Run applications that use multiple CPU threads within a node.
    </p>

  </div>


  <div class="advanced-job-card">

    <span>GPU</span>

    <strong>
      GPU Jobs
    </strong>

    <p>
      Request accelerator resources for CUDA and GPU-enabled applications.
    </p>

    <a href="../gpus/">
      GPU guide →
    </a>

  </div>

</div>


<div class="jobs-golden-rule">

  <div class="jobs-golden-mark">
    !
  </div>

  <div>

    <span class="roary-page-eyebrow">
      REMEMBER
    </span>

    <h3>
      Submit computational workloads through Slurm
    </h3>

    <p>
      Login nodes are shared access systems. CPU-intensive,
      memory-intensive, long-running, and GPU workloads should run
      on compute resources allocated by Slurm.
    </p>

  </div>

</div>


## Need Help?

If a job behaves unexpectedly, keep the **job ID**.

It is one of the most useful pieces of information when troubleshooting.

Useful commands include:

```bash
squeue -j JOBID
```

```bash
scontrol show job JOBID
```

```bash
sacct -j JOBID
```

If you still need assistance, email the HPC Admins:

[**hpcadmin@fiu.edu**](mailto:hpcadmin@fiu.edu)

Include:

- Your HPC username
- The Slurm job ID
- The command or job script you used
- Relevant `.out` and `.err` messages
- A short description of what you expected to happen

Never include your password.
