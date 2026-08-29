# Using Roary

<div class="using-roary-hero">

  <div class="using-roary-hero-content">

    <span class="roary-page-eyebrow">
      DAY-TO-DAY HPC
    </span>

    <h2>
      Your working environment on Roary
    </h2>

    <p>
      Once you have access to Roary, most of your work follows a common
      pattern: connect, prepare your environment, work with files and
      software, request compute resources, submit workloads, and review
      the results.
    </p>

  </div>

  <div class="using-roary-badge">

    <span>FIU</span>
    <strong>ROARY</strong>
    <small>WORKSPACE</small>

  </div>

</div>


## Start Your Roary Session

<div class="roary-access-strip">

  <div class="roary-access-method">

    <span class="roary-access-label">
      SSH
    </span>

    <strong>
      Command-Line Access
    </strong>

    <code>
      ssh USERNAME@hpclogin.fiu.edu
    </code>

    <a href="../getting-started/connect/">
      Connection guide →
    </a>

  </div>


  <div class="roary-access-divider">
    OR
  </div>


  <div class="roary-access-method">

    <span class="roary-access-label">
      WEB
    </span>

    <strong>
      Open OnDemand
    </strong>

    <code>
      https://hpclogin.fiu.edu
    </code>

    <a
      href="https://hpclogin.fiu.edu"
      target="_blank"
      rel="noopener">
      Open portal ↗
    </a>

  </div>

</div>


## The Roary Working Model

<div class="roary-working-flow">

  <div class="roary-working-step">

    <span>01</span>

    <div class="roary-working-symbol">
      LOGIN
    </div>

    <strong>
      Connect
    </strong>

    <p>
      Enter Roary through SSH or Open OnDemand.
    </p>

  </div>


  <div class="roary-working-arrow">
    →
  </div>


  <div class="roary-working-step">

    <span>02</span>

    <div class="roary-working-symbol">
      FILES
    </div>

    <strong>
      Prepare
    </strong>

    <p>
      Organize data, scripts, and working directories.
    </p>

  </div>


  <div class="roary-working-arrow">
    →
  </div>


  <div class="roary-working-step">

    <span>03</span>

    <div class="roary-working-symbol">
      MODULE
    </div>

    <strong>
      Software
    </strong>

    <p>
      Load applications and build your software environment.
    </p>

  </div>


  <div class="roary-working-arrow">
    →
  </div>


  <div class="roary-working-step roary-working-slurm">

    <span>04</span>

    <div class="roary-working-symbol">
      SLURM
    </div>

    <strong>
      Request Resources
    </strong>

    <p>
      Request CPU, memory, runtime, and GPU resources.
    </p>

  </div>


  <div class="roary-working-arrow">
    →
  </div>


  <div class="roary-working-step">

    <span>05</span>

    <div class="roary-working-symbol">
      RUN
    </div>

    <strong>
      Compute
    </strong>

    <p>
      Your workload executes on allocated compute resources.
    </p>

  </div>


  <div class="roary-working-arrow">
    →
  </div>


  <div class="roary-working-step">

    <span>06</span>

    <div class="roary-working-symbol">
      DATA
    </div>

    <strong>
      Results
    </strong>

    <p>
      Inspect output and review resource utilization.
    </p>

  </div>

</div>


## Common Tasks

<div class="using-roary-grid">

  <a class="using-roary-card" href="../getting-started/linux-basics/">

    <span class="using-card-code">
      SHELL
    </span>

    <strong>
      Work in Linux
    </strong>

    <p>
      Navigate directories, create files, copy data, inspect output,
      and use common command-line tools.
    </p>

    <small>
      Linux basics →
    </small>

  </a>


  <a class="using-roary-card" href="../software/">

    <span class="using-card-code">
      MODULE
    </span>

    <strong>
      Load Software
    </strong>

    <p>
      Discover installed applications and load the software environment
      required by your research workflow.
    </p>

    <small>
      Software guide →
    </small>

  </a>


  <a class="using-roary-card" href="../running-jobs/">

    <span class="using-card-code">
      SLURM
    </span>

    <strong>
      Submit Jobs
    </strong>

    <p>
      Request resources, submit batch jobs, monitor workloads,
      and understand scheduling.
    </p>

    <small>
      Running jobs →
    </small>

  </a>


  <a class="using-roary-card" href="../storage/">

    <span class="using-card-code">
      DATA
    </span>

    <strong>
      Manage Research Data
    </strong>

    <p>
      Learn where to store files, how quotas work, and how to
      transfer and organize research data.
    </p>

    <small>
      Storage guide →
    </small>

  </a>


  <a class="using-roary-card" href="../gpus/">

    <span class="using-card-code">
      GPU
    </span>

    <strong>
      Use GPU Resources
    </strong>

    <p>
      Request GPU resources and run CUDA-enabled or
      GPU-accelerated research applications.
    </p>

    <small>
      GPU computing →
    </small>

  </a>


  <a class="using-roary-card" href="../open-ondemand/">

    <span class="using-card-code">
      OOD
    </span>

    <strong>
      Interactive Computing
    </strong>

    <p>
      Use browser-based applications, development environments,
      desktops, and interactive research sessions.
    </p>

    <small>
      Open OnDemand →
    </small>

  </a>

</div>


## Essential Commands

These are some of the commands you will use frequently while working on Roary.

<div class="roary-command-grid">

  <div class="roary-command-card">

    <span>WHERE AM I?</span>

```bash
pwd
```

  </div>


  <div class="roary-command-card">

    <span>LIST FILES</span>

```bash
ls -lh
```

  </div>


  <div class="roary-command-card">

    <span>AVAILABLE SOFTWARE</span>

```bash
module avail
```

  </div>


  <div class="roary-command-card">

    <span>LOADED SOFTWARE</span>

```bash
module list
```

  </div>


  <div class="roary-command-card">

    <span>MY JOBS</span>

```bash
squeue -u $USER
```

  </div>


  <div class="roary-command-card">

    <span>SUBMIT JOB</span>

```bash
sbatch job.slurm
```

  </div>


  <div class="roary-command-card">

    <span>CANCEL JOB</span>

```bash
scancel JOBID
```

  </div>


  <div class="roary-command-card">

    <span>JOB HISTORY</span>

```bash
sacct -j JOBID
```

  </div>

</div>


## Login Environment vs Compute Resources

<div class="using-environment-grid">

  <div class="using-environment-card">

    <span class="using-env-code">
      LOGIN
    </span>

    <strong>
      Prepare your work
    </strong>

    <div class="using-env-items">

      <span>Edit files</span>
      <span>Transfer data</span>
      <span>Load modules</span>
      <span>Compile software</span>
      <span>Create job scripts</span>
      <span>Submit jobs</span>

    </div>

  </div>


  <div class="using-environment-arrow">
    →
  </div>


  <div class="using-environment-card using-compute-card">

    <span class="using-env-code">
      COMPUTE
    </span>

    <strong>
      Run your workload
    </strong>

    <div class="using-env-items">

      <span>CPU workloads</span>
      <span>Large-memory jobs</span>
      <span>GPU applications</span>
      <span>Parallel computing</span>
      <span>Long-running jobs</span>
      <span>Production analysis</span>

    </div>

  </div>

</div>

<div class="using-login-warning">

  <div class="using-warning-mark">
    !
  </div>

  <div>

    <span class="roary-page-eyebrow">
      IMPORTANT
    </span>

    <h3>
      The login environment is shared
    </h3>

    <p>
      Do not run CPU-intensive, memory-intensive, long-running,
      or GPU workloads directly in the login environment.
    </p>

    <p>
      Submit computational workloads through Slurm so that
      the appropriate compute resources are allocated to your job.
    </p>

    <a href="../getting-started/login-vs-compute/">
      Learn about login and compute nodes →
    </a>

  </div>

</div>


## Finding Software

Roary uses environment modules to make installed software available.

Start by listing available modules:

```bash
module avail
```

Search for a particular application:

```bash
module avail SOFTWARE_NAME
```

Load an application:

```bash
module load SOFTWARE_NAME
```

View your currently loaded modules:

```bash
module list
```

Unload a module:

```bash
module unload SOFTWARE_NAME
```

Remove all loaded modules:

```bash
module purge
```

For application-specific instructions:

[Software Guide :material-arrow-right:](../software/){ .md-button .md-button--primary }


## Working Interactively

There are two different ideas to keep separate:

<div class="interactive-method-grid">

  <div class="interactive-method-card">

    <span>
      SHELL
    </span>

    <strong>
      Login Shell
    </strong>

    <p>
      A normal SSH or Open OnDemand shell is intended for preparing work,
      not for large computational workloads.
    </p>

  </div>


  <div class="interactive-method-card interactive-compute-method">

    <span>
      COMPUTE
    </span>

    <strong>
      Interactive Compute Session
    </strong>

    <p>
      Interactive research work should use resources allocated through
      Slurm or a Slurm-backed Open OnDemand application.
    </p>

  </div>

</div>


## Monitor Your Work

Check your active jobs:

```bash
squeue -u $USER
```

Inspect a completed or running job:

```bash
sacct -j JOBID
```

For supported jobs, use the Roary JobUsage utility:

```bash
/home/share/bin/jobusage JOBID
```

JobUsage can help you inspect CPU, memory, and supported GPU utilization
for your workload.


## Where Should I Go Next?

<div class="using-next-grid">

  <a href="../running-jobs/">

    <span>JOBS</span>

    <strong>
      Running Jobs
    </strong>

    <small>
      Slurm, resources, monitoring →
    </small>

  </a>


  <a href="../software/">

    <span>SOFTWARE</span>

    <strong>
      Applications
    </strong>

    <small>
      Modules and environments →
    </small>

  </a>


  <a href="../storage/">

    <span>STORAGE</span>

    <strong>
      Data
    </strong>

    <small>
      Filesystems and quotas →
    </small>

  </a>


  <a href="../gpus/">

    <span>GPU</span>

    <strong>
      Accelerated Computing
    </strong>

    <small>
      GPU resources →
    </small>

  </a>


  <a href="../open-ondemand/">

    <span>OOD</span>

    <strong>
      Interactive Apps
    </strong>

    <small>
      Browser computing →
    </small>

  </a>


  <a href="../troubleshooting/">

    <span>HELP</span>

    <strong>
      Troubleshooting
    </strong>

    <small>
      Solve common problems →
    </small>

  </a>

</div>


## Need Help?

For Roary assistance, email the HPC Admins:

[**hpcadmin@fiu.edu**](mailto:hpcadmin@fiu.edu)
