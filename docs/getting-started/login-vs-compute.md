# Login Nodes vs Compute Nodes

<div class="nodes-intro-panel">

  <div>

    <span class="roary-page-eyebrow">
      ROARY COMPUTE ENVIRONMENT
    </span>

    <h2>
      Connect on the login layer. Compute on allocated resources.
    </h2>

    <p>
      One of the most important HPC concepts is understanding that the
      system you use to access Roary is not the system where large
      computational workloads should run.
    </p>

    <p>
      Users connect through <strong>hpclogin.fiu.edu</strong>, prepare
      their work in the login environment, and submit computational
      workloads to Slurm.
    </p>

  </div>

  <div class="nodes-intro-badge">

    <span>ROARY</span>
    <strong>SLURM</strong>
    <small>COMPUTE</small>

  </div>

</div>


## Two Different Environments

<div class="node-type-grid">

  <div class="node-type-card node-login-card">

    <div class="node-type-header">

      <span class="node-type-code">
        LOGIN
      </span>

      <span class="node-type-status">
        SHARED
      </span>

    </div>

    <h3>
      Login Environment
    </h3>

    <p>
      The login environment is where you prepare and manage your work.
      It is shared by many Roary users.
    </p>

    <div class="node-task-grid">

      <span>Editing files</span>
      <span>Managing directories</span>
      <span>Transferring data</span>
      <span>Loading modules</span>
      <span>Compiling software</span>
      <span>Preparing scripts</span>
      <span>Submitting jobs</span>
      <span>Checking job status</span>

    </div>

    <div class="node-command">

      <small>ACCESS</small>

      <code>
        ssh USERNAME@hpclogin.fiu.edu
      </code>

    </div>

  </div>


  <div class="node-type-card node-compute-card">

    <div class="node-type-header">

      <span class="node-type-code">
        COMPUTE
      </span>

      <span class="node-type-status">
        ALLOCATED
      </span>

    </div>

    <h3>
      Compute Nodes
    </h3>

    <p>
      Compute nodes are where CPU-intensive, memory-intensive,
      long-running, parallel, and GPU workloads should run.
    </p>

    <div class="node-task-grid">

      <span>Scientific applications</span>
      <span>Large calculations</span>
      <span>High-memory workloads</span>
      <span>Parallel jobs</span>
      <span>GPU workloads</span>
      <span>Production analysis</span>
      <span>Simulations</span>
      <span>Research pipelines</span>

    </div>

    <div class="node-command">

      <small>REQUEST</small>

      <code>
        sbatch job.slurm
      </code>

    </div>

  </div>

</div>


## How Your Work Moves Through Roary

When you connect to Roary, you enter the access environment first.
Your computational workload is then submitted to Slurm, which assigns
the appropriate compute resources.

<div class="node-flow">

  <div class="node-flow-step">

    <span>01</span>

    <div class="node-flow-symbol">
      YOU
    </div>

    <strong>
      Your Computer
    </strong>

    <small>
      Windows / macOS / Linux
    </small>

  </div>


  <div class="node-flow-arrow">

    <small>
      SSH / BROWSER
    </small>

    <div></div>

    <span>→</span>

  </div>


  <div class="node-flow-step node-flow-login">

    <span>02</span>

    <div class="node-flow-symbol">
      LOGIN
    </div>

    <strong>
      hpclogin.fiu.edu
    </strong>

    <small>
      Prepare and submit work
    </small>

  </div>


  <div class="node-flow-arrow">

    <small>
      SBATCH / SRUN
    </small>

    <div></div>

    <span>→</span>

  </div>


  <div class="node-flow-step node-flow-slurm">

    <span>03</span>

    <div class="node-flow-symbol">
      SLURM
    </div>

    <strong>
      Scheduler
    </strong>

    <small>
      Matches jobs to resources
    </small>

  </div>


  <div class="node-flow-arrow">

    <small>
      ALLOCATION
    </small>

    <div></div>

    <span>→</span>

  </div>


  <div class="node-flow-step node-flow-compute">

    <span>04</span>

    <div class="node-flow-symbol">
      NODE
    </div>

    <strong>
      Compute Resources
    </strong>

    <small>
      CPU / Memory / GPU
    </small>

  </div>


  <div class="node-flow-arrow">

    <small>
      OUTPUT
    </small>

    <div></div>

    <span>→</span>

  </div>


  <div class="node-flow-step">

    <span>05</span>

    <div class="node-flow-symbol">
      DATA
    </div>

    <strong>
      Results
    </strong>

    <small>
      Output and job data
    </small>

  </div>

</div>


## Why Does Roary Use Slurm?

Roary is a shared computing environment. Many researchers may need
CPU, memory, GPU, and other resources at the same time.

Slurm manages those requests.

<div class="scheduler-grid">

  <div class="scheduler-card">

    <span>QUEUE</span>

    <strong>
      Job Scheduling
    </strong>

    <p>
      Determines when a submitted job can begin running.
    </p>

  </div>


  <div class="scheduler-card">

    <span>CPU</span>

    <strong>
      Processor Allocation
    </strong>

    <p>
      Assigns the number of CPUs requested by your workload.
    </p>

  </div>


  <div class="scheduler-card">

    <span>RAM</span>

    <strong>
      Memory Allocation
    </strong>

    <p>
      Reserves the amount of memory requested by the job.
    </p>

  </div>


  <div class="scheduler-card">

    <span>GPU</span>

    <strong>
      Accelerator Allocation
    </strong>

    <p>
      Assigns GPU resources when requested and available.
    </p>

  </div>


  <div class="scheduler-card">

    <span>TIME</span>

    <strong>
      Runtime Limits
    </strong>

    <p>
      Tracks the maximum runtime requested for the allocation.
    </p>

  </div>


  <div class="scheduler-card">

    <span>NODE</span>

    <strong>
      Resource Matching
    </strong>

    <p>
      Selects compute nodes capable of satisfying the request.
    </p>

  </div>

</div>


## Example: Submitting a Job

You prepare your job script while connected to the Roary login environment.

Then submit it:

```bash
sbatch analysis.slurm
```

Slurm responds with a job ID:

```text
Submitted batch job 123456
```

<div class="job-submit-explainer">

  <div>

    <span class="job-submit-number">
      1
    </span>

    <strong>
      Your shell stays in the login environment
    </strong>

    <p>
      Submitting the job does not move your terminal session to a compute node.
    </p>

  </div>


  <div>

    <span class="job-submit-number">
      2
    </span>

    <strong>
      Slurm places the job in the scheduler
    </strong>

    <p>
      The job may begin immediately or wait until the requested resources are available.
    </p>

  </div>


  <div>

    <span class="job-submit-number">
      3
    </span>

    <strong>
      The workload runs on allocated compute resources
    </strong>

    <p>
      Your application executes separately from your login session.
    </p>

  </div>

</div>


## Check Your Jobs

To view your current jobs:

```bash
squeue -u $USER
```

For example:

```text
JOBID    PARTITION    NAME        USER      ST    TIME
123456   ...          analysis    username  R     0:12
```

Common states include:

<div class="job-state-grid">

  <div>
    <span>PD</span>
    <strong>Pending</strong>
  </div>

  <div>
    <span>R</span>
    <strong>Running</strong>
  </div>

  <div>
    <span>CG</span>
    <strong>Completing</strong>
  </div>

  <div>
    <span>CD</span>
    <strong>Completed</strong>
  </div>

</div>


## Monitor Resource Usage

For supported running jobs, Roary provides the JobUsage utility:

```bash
/home/share/bin/jobusage JOBID
```

Example:

```bash
/home/share/bin/jobusage 123456
```

JobUsage can help show whether your workload is efficiently using
the resources requested from Slurm.


<div class="node-golden-rule">

  <div class="node-rule-mark">
    !
  </div>

  <div>

    <span class="roary-page-eyebrow">
      GOLDEN RULE
    </span>

    <h3>
      Prepare on the login nodes. Compute through Slurm.
    </h3>

    <p>
      If a command will use significant CPU, memory, GPU resources,
      or run for an extended period of time, do not run it directly
      in the login environment.
    </p>

    <p>
      Submit the workload through Slurm so that Roary can allocate
      the appropriate compute resources.
    </p>

  </div>

</div>


## Need Help?

If you are unsure whether something should run on a login node or
through Slurm, email the HPC Admins:

[**hpcadmin@fiu.edu**](mailto:hpcadmin@fiu.edu)


## Next Step

[Learn Basic Linux Commands :material-arrow-right:](linux-basics.md){ .md-button .md-button--primary }
