# Getting Started with Roary

Welcome to the **Roary High Performance Computing environment**.

This guide is designed for users who may be completely new to HPC. It will take you from obtaining access to successfully running and monitoring your first computational job.

---

## Start Here

<div class="grid cards" markdown>

-   :material-information-outline:{ .lg .middle }

    **What is Roary?**

    Learn what Roary is, what HPC is used for, and how the cluster is organized.

    [:octicons-arrow-right-24: Learn about Roary](what-is-roary.md)

-   :material-account-plus:{ .lg .middle }

    **Request Access**

    Learn how Roary accounts and research-group access work.

    [:octicons-arrow-right-24: Request access](request-access.md)

-   :material-lan-connect:{ .lg .middle }

    **Connect to Roary**

    Connect using SSH from Windows, macOS, or Linux.

    [:octicons-arrow-right-24: Connect](connect.md)

-   :material-web:{ .lg .middle }

    **Open OnDemand**

    Access Roary using your web browser.

    [:octicons-arrow-right-24: Browser access](open-ondemand.md)

-   :material-server-network:{ .lg .middle }

    **Understand the Cluster**

    Learn the difference between login nodes, compute nodes, and Slurm.

    [:octicons-arrow-right-24: How Roary works](login-vs-compute.md)

-   :material-console-line:{ .lg .middle }

    **Linux Basics**

    Learn the Linux commands you will use most often on Roary.

    [:octicons-arrow-right-24: Linux basics](linux-basics.md)

-   :material-rocket-launch:{ .lg .middle }

    **Run Your First Job**

    Create and submit your first Slurm batch job.

    [:octicons-arrow-right-24: First job](first-job.md)

-   :material-map-marker-path:{ .lg .middle }

    **Where to Go Next**

    Continue to software, storage, GPUs, Open OnDemand, and advanced jobs.

    [:octicons-arrow-right-24: Next steps](next-steps.md)

</div>

---

## Your First Roary Workflow

Your work moves through several layers of the Roary environment. You connect to the cluster, submit your workload to Slurm, and Slurm assigns the appropriate compute resources.

<div class="gs-workflow">

  <div class="gs-workflow-step">

    <div class="gs-step-top">
      <span class="gs-step-number">01</span>
      <span class="gs-step-type">LOCAL</span>
    </div>

    <div class="gs-step-symbol">YOU</div>

    <strong>Your Computer</strong>

    <p>
      Start from your Windows, macOS, or Linux workstation.
    </p>

  </div>


  <div class="gs-flow-connector">
    <span>SSH / BROWSER</span>
    <div class="gs-flow-line"></div>
    <div class="gs-flow-arrow">→</div>
  </div>


  <div class="gs-workflow-step">

    <div class="gs-step-top">
      <span class="gs-step-number">02</span>
      <span class="gs-step-type">ACCESS</span>
    </div>

    <div class="gs-step-symbol">LOGIN</div>

    <strong>Roary Access</strong>

    <p>
      Connect through a login node or use Open OnDemand.
    </p>

  </div>


  <div class="gs-flow-connector">
    <span>SBATCH / SRUN</span>
    <div class="gs-flow-line"></div>
    <div class="gs-flow-arrow">→</div>
  </div>


  <div class="gs-workflow-step gs-step-slurm">

    <div class="gs-step-top">
      <span class="gs-step-number">03</span>
      <span class="gs-step-type">SCHEDULER</span>
    </div>

    <div class="gs-step-symbol">SLURM</div>

    <strong>Slurm Scheduler</strong>

    <p>
      Evaluates your request and selects available resources.
    </p>

  </div>


  <div class="gs-flow-connector">
    <span>ALLOCATION</span>
    <div class="gs-flow-line"></div>
    <div class="gs-flow-arrow">→</div>
  </div>


  <div class="gs-workflow-step">

    <div class="gs-step-top">
      <span class="gs-step-number">04</span>
      <span class="gs-step-type">COMPUTE</span>
    </div>

    <div class="gs-step-symbol">NODE</div>

    <strong>Compute Resources</strong>

    <p>
      Your workload runs using the requested CPU, memory, or GPU resources.
    </p>

  </div>


  <div class="gs-flow-connector">
    <span>OUTPUT</span>
    <div class="gs-flow-line"></div>
    <div class="gs-flow-arrow">→</div>
  </div>


  <div class="gs-workflow-step gs-step-results">

    <div class="gs-step-top">
      <span class="gs-step-number">05</span>
      <span class="gs-step-type">RESULTS</span>
    </div>

    <div class="gs-step-symbol">DATA</div>

    <strong>Your Results</strong>

    <p>
      Review output files, job status, and resource utilization.
    </p>

  </div>

</div>


<div class="gs-login-alert">

  <div class="gs-alert-marker">
    !
  </div>

  <div class="gs-alert-content">

    <span class="gs-alert-eyebrow">
      IMPORTANT
    </span>

    <h3>
      Login nodes are for preparing work — not running it
    </h3>

    <p>
      Use login nodes for editing files, transferring data, managing
      software environments, compiling code, and submitting jobs.
    </p>

    <p>
      CPU-intensive, memory-intensive, long-running, and GPU workloads
      must run on compute resources allocated through Slurm.
    </p>

    <a href="login-vs-compute.md">
      Learn about login and compute nodes →
    </a>

  </div>

</div>

---

## Getting Started Checklist

Before you begin real work on Roary, make sure you can complete the following tasks.

<div class="gs-checklist-panel">

  <div class="gs-checklist-header">

    <div>
      <span class="gs-checklist-eyebrow">ROARY READINESS</span>
      <h3>Launch Checklist</h3>
      <p>
        These are the core skills every new Roary user should complete before
        moving on to larger research workloads.
      </p>
    </div>

    <div class="gs-checklist-progress">
      <span>10</span>
      <small>CORE STEPS</small>
    </div>

  </div>


  <div class="gs-checklist-grid">

    <a class="gs-check-item" href="request-access.md">
      <span class="gs-check-num">01</span>
      <div class="gs-check-body">
        <strong>Obtain a Roary HPC account</strong>
        <p>Make sure your access and research-group permissions are ready.</p>
      </div>
      <span class="gs-check-link">Open →</span>
    </a>

    <a class="gs-check-item" href="connect.md">
      <span class="gs-check-num">02</span>
      <div class="gs-check-body">
        <strong>Connect using SSH or Open OnDemand</strong>
        <p>Be able to reach the Roary environment from your system.</p>
      </div>
      <span class="gs-check-link">Open →</span>
    </a>

    <a class="gs-check-item" href="login-vs-compute.md">
      <span class="gs-check-num">03</span>
      <div class="gs-check-body">
        <strong>Understand login and compute nodes</strong>
        <p>Know where to prepare work and where workloads actually run.</p>
      </div>
      <span class="gs-check-link">Open →</span>
    </a>

    <a class="gs-check-item" href="linux-basics.md">
      <span class="gs-check-num">04</span>
      <div class="gs-check-body">
        <strong>Learn basic Linux commands</strong>
        <p>Navigate files, create directories, and work comfortably in the shell.</p>
      </div>
      <span class="gs-check-link">Open →</span>
    </a>

    <a class="gs-check-item" href="../software/">
      <span class="gs-check-num">05</span>
      <div class="gs-check-body">
        <strong>Learn how software modules work</strong>
        <p>Understand how to discover and load software on Roary.</p>
      </div>
      <span class="gs-check-link">Open →</span>
    </a>

    <a class="gs-check-item" href="first-job.md">
      <span class="gs-check-num">06</span>
      <div class="gs-check-body">
        <strong>Create a Slurm job script</strong>
        <p>Write a simple batch script that requests resources correctly.</p>
      </div>
      <span class="gs-check-link">Open →</span>
    </a>

    <a class="gs-check-item" href="first-job.md">
      <span class="gs-check-num">07</span>
      <div class="gs-check-body">
        <strong>Submit your job</strong>
        <p>Use <code>sbatch</code> to send your workload to Slurm.</p>
      </div>
      <span class="gs-check-link">Open →</span>
    </a>

    <a class="gs-check-item" href="../running-jobs/">
      <span class="gs-check-num">08</span>
      <div class="gs-check-body">
        <strong>Monitor your job</strong>
        <p>Check queue status, runtime, and whether the job is pending or running.</p>
      </div>
      <span class="gs-check-link">Open →</span>
    </a>

    <a class="gs-check-item" href="first-job.md">
      <span class="gs-check-num">09</span>
      <div class="gs-check-body">
        <strong>Check your output</strong>
        <p>Read output and error files after the job finishes.</p>
      </div>
      <span class="gs-check-link">Open →</span>
    </a>

    <a class="gs-check-item" href="first-job.md">
      <span class="gs-check-num">10</span>
      <div class="gs-check-body">
        <strong>Review resource usage</strong>
        <p>Inspect CPU, memory, and other resource utilization for your job.</p>
      </div>
      <span class="gs-check-link">Open →</span>
    </a>

  </div>

</div>

<div class="gs-checklist-note">
  Once you can complete these steps, you are ready to begin using Roary for real research workloads with confidence.
</div>
