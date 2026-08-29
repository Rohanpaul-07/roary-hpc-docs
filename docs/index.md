---
hide:
  - navigation
  - toc
---

<div class="institutional-brand">

  <img
    src="assets/images/fiu-ircc-banner.png"
    alt="Florida International University Instructional and Research Computing Center">

</div>



<div class="fiu-hero">

  <div class="fiu-hero-grid">

    <div class="fiu-hero-content">

      <div class="fiu-eyebrow">
        FIU RESEARCH COMPUTING
      </div>

      <h1>ROARY</h1>

      <h2>High Performance Computing</h2>

      <p class="hero-lead">
        Powering research through advanced CPU, GPU, high-memory,
        interactive computing, and research data infrastructure.
      </p>

      <div class="hero-tags">
        <span>CPU COMPUTE</span>
        <span>GPU ACCELERATION</span>
        <span>HIGH MEMORY</span>
        <span>RESEARCH STORAGE</span>
      </div>

      <div class="hero-actions">
        <a href="getting-started/" class="md-button fiu-primary-button">
          Get Started →
        </a>

        <a href="open-ondemand/" class="md-button fiu-secondary-button">
          Open OnDemand ↗
        </a>
      </div>

    </div>

    <div class="hero-visual">

      <div class="compute-core">

        <div class="core-ring ring-one"></div>
        <div class="core-ring ring-two"></div>
        <div class="core-ring ring-three"></div>

        <div class="core-center">
          <span class="core-small">FIU</span>
          <strong>ROARY</strong>
          <span>HPC</span>
        </div>

        <div class="orbit-node orbit-one"></div>
        <div class="orbit-node orbit-two"></div>
        <div class="orbit-node orbit-three"></div>

      </div>

    </div>

  </div>

  <div class="energy-line"></div>

</div>


<div class="home-section" markdown>

<div class="section-eyebrow">EXPLORE ROARY</div>

## What do you want to do?

<p class="section-intro">
Everything you need to access Roary, submit computational workloads,
manage research data, use software, and troubleshoot your jobs.
</p>

<div class="grid cards roary-cards" markdown>

-   :material-rocket-launch:{ .lg .middle }

    ### Getting Started

    New to HPC? Start with account access, connecting to Roary,
    and submitting your first job.

    [:octicons-arrow-right-24: **Start here**](getting-started/)

-   :material-console:{ .lg .middle }

    ### Run a Job

    Learn Slurm, request CPUs and memory, submit workloads,
    monitor jobs, and understand scheduling.

    [:octicons-arrow-right-24: **Running jobs**](running-jobs/)

-   :material-web:{ .lg .middle }

    ### Open OnDemand

    Access Roary from your browser with shells, interactive desktops,
    development environments, and research applications.

    [:octicons-arrow-right-24: **Open OnDemand**](open-ondemand/)

-   :material-database:{ .lg .middle }

    ### Storage & Data

    Understand home, scratch, research storage, quotas,
    permissions, and transferring large datasets.

    [:octicons-arrow-right-24: **Storage guide**](storage/)

-   :material-package-variant-closed:{ .lg .middle }

    ### Software

    Discover and load Python, R, MATLAB, compilers,
    scientific applications, containers, and more.

    [:octicons-arrow-right-24: **Browse software**](software/)

-   :material-expansion-card-variant:{ .lg .middle }

    ### GPU Computing

    Run accelerated workloads using Roary GPU resources,
    CUDA, and GPU-enabled applications.

    [:octicons-arrow-right-24: **GPU guide**](gpus/)

-   :material-code-braces:{ .lg .middle }

    ### Examples

    Start from working Slurm examples for CPU, GPU,
    Python, R, MATLAB, MPI, OpenMP, and job arrays.

    [:octicons-arrow-right-24: **View examples**](examples/)

-   :material-lifebuoy:{ .lg .middle }

    ### Troubleshooting

    Find answers for pending jobs, memory failures,
    quota issues, software problems, and OOD sessions.

    [:octicons-arrow-right-24: **Get help**](troubleshooting/)

</div>

</div>


<div class="dark-feature-section">

  <div class="section-eyebrow light">
    NEW TO HIGH PERFORMANCE COMPUTING?
  </div>

  <h2>Your path to your first Roary job</h2>

  <div class="journey">

    <div class="journey-step">
      <div class="journey-number">01</div>
      <div class="journey-content">
        <strong>ACCESS</strong>
        <span>Get your Roary account</span>
      </div>
    </div>

    <div class="journey-connector"></div>

    <div class="journey-step">
      <div class="journey-number">02</div>
      <div class="journey-content">
        <strong>CONNECT</strong>
        <span>SSH or Open OnDemand</span>
      </div>
    </div>

    <div class="journey-connector"></div>

    <div class="journey-step">
      <div class="journey-number">03</div>
      <div class="journey-content">
        <strong>SUBMIT</strong>
        <span>Launch a Slurm job</span>
      </div>
    </div>

    <div class="journey-connector"></div>

    <div class="journey-step">
      <div class="journey-number">04</div>
      <div class="journey-content">
        <strong>MONITOR</strong>
        <span>Track resource usage</span>
      </div>
    </div>

  </div>

  <div class="journey-button">
    <a href="getting-started/" class="md-button fiu-gold-button">
      Start the Getting Started Guide →
    </a>
  </div>

</div>


<div class="home-section" markdown>

<div class="section-eyebrow">ESSENTIAL COMMANDS</div>

## Roary quick start

<div class="command-grid">

<div class="command-card" markdown>

<span class="command-label">CONNECT</span>

```bash
ssh USERNAME@LOGIN_HOST
