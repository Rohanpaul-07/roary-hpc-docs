# Software

<div class="software-hero">

  <div class="software-hero-content">

    <span class="roary-page-eyebrow">
      ROARY SOFTWARE
    </span>

    <h2>
      Scientific applications and research software on Roary
    </h2>

    <p>
      Roary provides centrally installed research software through
      Environment Modules together with tools for creating your own
      project-specific software environments.
    </p>

    <p>
      Use the Software section on the left to find application-specific
      instructions, example Slurm jobs, resource guidance, and
      troubleshooting information.
    </p>

  </div>

  <div class="software-hero-badge">

    <span>ROARY</span>
    <strong>SOFTWARE</strong>
    <small>HPC</small>

  </div>

</div>


## Find Software on Roary

List currently available software:

```bash
module avail
```

Find a particular application:

```bash
module avail SOFTWARE_NAME
```

Examples:

```bash
module avail python
```

```bash
module avail R
```

```bash
module avail matlab
```

```bash
module avail cuda
```

```bash
module avail spades
```

Load the exact module shown by Roary:

```bash
module load SOFTWARE/VERSION
```

Check what you currently have loaded:

```bash
module list
```

Inspect a module:

```bash
module show SOFTWARE/VERSION
```

Start with a clean module environment:

```bash
module purge
```


<div class="software-command-tip">

  <span class="roary-page-eyebrow">
    SOFTWARE DISCOVERY
  </span>

  <h3>
    Use module avail
  </h3>

  <p>
    Software versions and installed applications can change.
    Always use <code>module avail SOFTWARE_NAME</code> to check the
    software currently available on Roary.
  </p>

</div>


## Software Documentation

<div class="software-area-grid">

  <div>
    <span>MODULES</span>
    <strong>Environment Modules</strong>
  </div>

  <div>
    <span>LANG</span>
    <strong>Languages & Environments</strong>
  </div>

  <div>
    <span>AI</span>
    <strong>AI & Machine Learning</strong>
  </div>

  <div>
    <span>BIO</span>
    <strong>Bioinformatics & Genomics</strong>
  </div>

  <div>
    <span>CHEM</span>
    <strong>Chemistry & Molecular Modeling</strong>
  </div>

  <div>
    <span>MPI</span>
    <strong>Compilers & Parallel Computing</strong>
  </div>

  <div>
    <span>DATA</span>
    <strong>Math, Statistics & Data Science</strong>
  </div>

  <div>
    <span>ENG</span>
    <strong>Engineering & Simulation</strong>
  </div>

  <div>
    <span>VIS</span>
    <strong>Visualization</strong>
  </div>

  <div>
    <span>APP</span>
    <strong>Containers</strong>
  </div>

  <div>
    <span>FLOW</span>
    <strong>Workflow Managers</strong>
  </div>

  <div>
    <span>OOD</span>
    <strong>Interactive Software</strong>
  </div>

</div>


## Need Software That Is Not Available?

First check:

```bash
module avail SOFTWARE_NAME
```

If the application is not available, email the HPC Admins:

[**hpcadmin@fiu.edu**](mailto:hpcadmin@fiu.edu)

Include:

- Your HPC username
- Research group or PI
- Software name
- Required version
- Official software website
- Whether the software requires a license
- Any known compiler, MPI, CUDA, or library requirements


## Start Here

[Environment Modules :material-arrow-right:](modules/){ .md-button .md-button--primary }
