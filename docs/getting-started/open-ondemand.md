# Open OnDemand

<div class="ood-intro-panel">

  <div class="ood-intro-content">

    <span class="roary-page-eyebrow">
      BROWSER ACCESS TO ROARY
    </span>

    <h2>
      Use Roary directly from your web browser
    </h2>

    <p>
      Open OnDemand provides a graphical, browser-based way to access
      Roary without requiring a traditional SSH terminal for every task.
    </p>

    <p>
      You can manage files, open a shell, monitor jobs, and launch
      interactive research applications from one interface.
    </p>

    <a
      href="https://hpclogin.fiu.edu"
      target="_blank"
      rel="noopener"
      class="md-button fiu-primary-button">
      Open Roary Open OnDemand ↗
    </a>

  </div>


  <div class="ood-portal-badge">

    <span>FIU</span>

    <strong>OOD</strong>

    <small>ROARY</small>

  </div>

</div>


## Accessing Open OnDemand

Open a supported web browser and go to:

<div class="ood-url-panel">

  <div>

    <span class="roary-page-eyebrow">
      ROARY WEB PORTAL
    </span>

    <strong>
      https://hpclogin.fiu.edu
    </strong>

  </div>

  <a
    href="https://hpclogin.fiu.edu"
    target="_blank"
    rel="noopener">
    Open Portal ↗
  </a>

</div>

Sign in using your authorized FIU/HPC credentials.


## Open OnDemand Dashboard

After signing in successfully, you will arrive at the Roary Open OnDemand
dashboard.

<div class="doc-screenshot">

  <img
    src="../../assets/screenshots/ood/roary-ood-dashboard.png"
    alt="Roary Open OnDemand dashboard">

  <div class="doc-screenshot-caption">
    Roary Open OnDemand dashboard at
    <strong>hpclogin.fiu.edu</strong>.
  </div>

</div>

The top navigation provides access to the major Open OnDemand features,
including **Files**, **Jobs**, **Clusters**, **Interactive Apps**, and
**My Interactive Sessions**.

The Roary dashboard may also display HPC notices and locally provided
HPC tools.


## What Can You Do in Open OnDemand?

<div class="ood-main-grid">

  <div class="ood-main-card">

    <span class="ood-card-code">
      FILES
    </span>

    <strong>
      Manage Files
    </strong>

    <p>
      Browse your directories, upload and download files,
      create folders, rename files, and manage research data.
    </p>

  </div>


  <div class="ood-main-card">

    <span class="ood-card-code">
      SHELL
    </span>

    <strong>
      Open a Terminal
    </strong>

    <p>
      Start a browser-based shell for working with files,
      software modules, scripts, and Slurm commands.
    </p>

  </div>


  <div class="ood-main-card">

    <span class="ood-card-code">
      JOBS
    </span>

    <strong>
      Monitor Jobs
    </strong>

    <p>
      View active jobs and inspect the status of workloads
      submitted to the Slurm scheduler.
    </p>

  </div>


  <div class="ood-main-card">

    <span class="ood-card-code">
      APPS
    </span>

    <strong>
      Interactive Applications
    </strong>

    <p>
      Launch supported research applications through
      scheduled compute resources.
    </p>

  </div>

</div>


## Files

The Open OnDemand Files interface provides a graphical way to work with
your files and directories.

<div class="ood-file-actions">

  <span>Browse Directories</span>
  <span>Upload Files</span>
  <span>Download Files</span>
  <span>Create Directories</span>
  <span>Rename Files</span>
  <span>Delete Files</span>
  <span>Edit Text Files</span>

</div>

!!! warning "Be careful when deleting files"

    Files deleted from HPC storage may not behave like files moved to a
    desktop recycle bin. Always verify the path and file before deleting it.


## Shell Access

Open OnDemand provides a browser-based shell that behaves similarly to an SSH session.

<div class="ood-shell-panel">

  <div class="ood-shell-code">

    <span>&gt;_</span>

  </div>

  <div>

    <span class="roary-page-eyebrow">
      COMMAND LINE
    </span>

    <h3>
      Work from the browser
    </h3>

    <p>
      Use the shell to navigate files, load software modules,
      prepare scripts, submit jobs, and run Slurm commands.
    </p>

  </div>

</div>

For example:

```bash
squeue -u $USER
```

or:

```bash
module avail
```

!!! warning "The shell is not a compute allocation"

    Opening a shell in Open OnDemand does not automatically allocate
    compute resources.

    CPU-intensive, memory-intensive, long-running, and GPU workloads
    should be submitted through Slurm.


## Interactive Applications

Interactive applications are one of the main advantages of Open OnDemand.

Instead of manually configuring a remote graphical application, you can
request resources through a web form and let Open OnDemand submit the
corresponding Slurm job.

<div class="ood-app-grid">

  <div class="ood-app-card">

    <span>DESKTOP</span>

    <strong>
      Interactive Desktop
    </strong>

    <p>
      Launch a graphical Linux desktop on scheduled compute resources.
    </p>

  </div>


  <div class="ood-app-card">

    <span>JUPYTER</span>

    <strong>
      Jupyter
    </strong>

    <p>
      Run notebook-based Python and scientific computing workflows.
    </p>

  </div>


  <div class="ood-app-card">

    <span>RSTUDIO</span>

    <strong>
      RStudio
    </strong>

    <p>
      Use a browser-based R development environment on Roary.
    </p>

  </div>


  <div class="ood-app-card">

    <span>VSCODE</span>

    <strong>
      VS Code
    </strong>

    <p>
      Launch a browser-accessible development environment on compute resources.
    </p>

  </div>


  <div class="ood-app-card">

    <span>MATLAB</span>

    <strong>
      MATLAB
    </strong>

    <p>
      Use MATLAB interactively when the application is available to your account.
    </p>

  </div>


  <div class="ood-app-card">

    <span>MORE</span>

    <strong>
      Additional Apps
    </strong>

    <p>
      Other interactive applications may be available depending on
      software, licensing, and your access.
    </p>

  </div>

</div>


## How an Interactive Session Works

When you launch an interactive application, Open OnDemand submits a job to Slurm on your behalf.

<div class="ood-session-flow">

  <div class="ood-session-step">

    <span>01</span>

    <strong>
      Open Browser
    </strong>

    <small>
      hpclogin.fiu.edu
    </small>

  </div>


  <div class="ood-session-arrow">
    →
  </div>


  <div class="ood-session-step">

    <span>02</span>

    <strong>
      Choose an App
    </strong>

    <small>
      Desktop / Jupyter / RStudio / VS Code
    </small>

  </div>


  <div class="ood-session-arrow">
    →
  </div>


  <div class="ood-session-step">

    <span>03</span>

    <strong>
      Request Resources
    </strong>

    <small>
      CPU / Memory / Time / GPU
    </small>

  </div>


  <div class="ood-session-arrow">
    →
  </div>


  <div class="ood-session-step ood-session-slurm">

    <span>04</span>

    <strong>
      Slurm
    </strong>

    <small>
      Schedules the session
    </small>

  </div>


  <div class="ood-session-arrow">
    →
  </div>


  <div class="ood-session-step">

    <span>05</span>

    <strong>
      Compute Node
    </strong>

    <small>
      Interactive workload runs here
    </small>

  </div>

</div>


## Choosing Resources

The options shown depend on the application, but an interactive session may ask you to choose resources such as:

<div class="ood-resource-grid">

  <div class="ood-resource-item">
    <span>CPU</span>
    <strong>Processors</strong>
  </div>

  <div class="ood-resource-item">
    <span>RAM</span>
    <strong>Memory</strong>
  </div>

  <div class="ood-resource-item">
    <span>TIME</span>
    <strong>Runtime</strong>
  </div>

  <div class="ood-resource-item">
    <span>QUEUE</span>
    <strong>Partition / QoS</strong>
  </div>

  <div class="ood-resource-item">
    <span>GPU</span>
    <strong>GPU Resources</strong>
  </div>

</div>


<div class="ood-resource-warning">

  <div class="ood-warning-mark">
    !
  </div>

  <div>

    <span class="roary-page-eyebrow">
      RESOURCE REQUESTS
    </span>

    <h3>
      Request what your workload actually needs
    </h3>

    <p>
      Larger resource requests are not automatically better.
      Requests for more CPUs, memory, GPUs, or longer runtime may
      have fewer immediately available resources and can take longer
      to schedule.
    </p>

  </div>

</div>


## Open OnDemand or SSH?

Both methods provide access to the same Roary environment, but they are useful for different styles of work.

<div class="ood-vs-grid">

  <div class="ood-vs-card">

    <span class="ood-vs-label">
      SSH
    </span>

    <strong>
      Best for command-line workflows
    </strong>

    <p>
      Use SSH when you prefer a terminal, automate workflows,
      transfer data with command-line tools, or submit Slurm jobs directly.
    </p>

    <code>
      ssh USERNAME@hpclogin.fiu.edu
    </code>

  </div>


  <div class="ood-vs-card ood-vs-card-highlight">

    <span class="ood-vs-label">
      OPEN ONDEMAND
    </span>

    <strong>
      Best for browser-based and interactive work
    </strong>

    <p>
      Use Open OnDemand when you want graphical file management,
      browser shells, job views, desktops, or interactive applications.
    </p>

    <code>
      https://hpclogin.fiu.edu
    </code>

  </div>

</div>

You do not have to choose only one. Many Roary users use both depending on the task.


## Need Help?

If you cannot access Open OnDemand or an interactive application does not start,
email the HPC Admins:

[**hpcadmin@fiu.edu**](mailto:hpcadmin@fiu.edu)

When requesting assistance, include:

- Your HPC username
- The application you were trying to launch
- The approximate time of the problem
- Any Slurm job ID shown
- The complete error message

Never send your password.


## Next Step

[Understand Login and Compute Nodes :material-arrow-right:](login-vs-compute.md){ .md-button .md-button--primary }
