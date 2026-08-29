# Environment Modules

<div class="module-hero">

  <div class="module-hero-content">

    <span class="roary-page-eyebrow">
      ROARY SOFTWARE ENVIRONMENT
    </span>

    <h2>
      Find, load, and manage software on Roary
    </h2>

    <p>
      Roary uses Environment Modules to provide centrally installed
      compilers, libraries, scientific applications, and development tools.
    </p>

    <p>
      The module system allows multiple software versions to coexist
      while letting each user select the version required for a workflow.
    </p>

  </div>

  <div class="module-hero-badge">

    <span>ROARY</span>
    <strong>MODULES</strong>
    <small>5.5.0</small>

  </div>

</div>


## Environment Modules on Roary

Roary uses **Environment Modules 5.5.0**.

Check the version with:

```bash
module --version
```

or:

```bash
module -V
```

Environment Modules modifies the current shell environment when software
is loaded. Modulefiles commonly modify variables such as `PATH`,
`MANPATH`, and library-related environment variables. :contentReference[oaicite:1]{index=1}


!!! info "Environment Modules are not Python modules"

    The Linux `module` command used on Roary is a software-environment
    management system.

    It is different from Python modules, R packages, Perl modules,
    or Java libraries.


## Why Roary Uses Modules

Research software often requires different:

- Application versions
- Compiler versions
- MPI implementations
- CUDA versions
- Scientific libraries
- Runtime environments

Modules allow those software stacks to coexist without requiring users
to manually modify system paths.

For example:

```bash
module load gcc/VERSION
```

changes your current shell so that the selected GCC installation becomes
available.

The Environment Modules project specifically supports dynamically loading,
switching, and unloading application versions. :contentReference[oaicite:2]{index=2}


## Essential Commands

<div class="module-command-grid">

  <div class="module-command-card">

    <span>AVAILABLE</span>

    <strong>
      List available modules
    </strong>

```bash
module avail
```

  </div>


  <div class="module-command-card">

    <span>FIND</span>

    <strong>
      Find an application
    </strong>

```bash
module avail NAME
```

  </div>


  <div class="module-command-card">

    <span>LOAD</span>

    <strong>
      Load software
    </strong>

```bash
module load NAME/VERSION
```

  </div>


  <div class="module-command-card">

    <span>LOADED</span>

    <strong>
      Show loaded modules
    </strong>

```bash
module list
```

  </div>


  <div class="module-command-card">

    <span>DETAILS</span>

    <strong>
      Inspect a module
    </strong>

```bash
module show NAME/VERSION
```

  </div>


  <div class="module-command-card">

    <span>REMOVE</span>

    <strong>
      Unload a module
    </strong>

```bash
module unload NAME
```

  </div>


  <div class="module-command-card">

    <span>CHANGE</span>

    <strong>
      Switch versions
    </strong>

```bash
module switch OLD NEW
```

  </div>


  <div class="module-command-card">

    <span>CLEAN</span>

    <strong>
      Unload all modules
    </strong>

```bash
module purge
```

  </div>

</div>


## Finding Software

### List Everything Currently Available

Run:

```bash
module avail
```

This displays the modulefiles currently available through your Roary
module search path.


### Find a Particular Application

Use:

```bash
module avail NAME
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

This is the primary software-discovery command users should use on Roary.


### Case-Insensitive Search

If you are unsure about capitalization:

```bash
module -i avail NAME
```

Example:

```bash
module -i avail python
```

Environment Modules 5.5 supports the `--icase` / `-i` option for
case-insensitive matching. :contentReference[oaicite:3]{index=3}


### Show the Highest Available Version

You can display the highest numerically sorted version matching a module:

```bash
module -L avail NAME
```

Example:

```bash
module -L avail gcc
```

The `-L` / `--latest` option is supported by Environment Modules 5.5
for `module avail`. :contentReference[oaicite:4]{index=4}


## `module search` — What It Actually Does

Roary's Environment Modules release also supports:

```bash
module search STRING
```

However, this command searches the **`module-whatis` descriptive text**
inside modulefiles. It is not the primary command for finding a module
by its filename. :contentReference[oaicite:5]{index=5}

For example:

```bash
module search chemistry
```

may return modules whose descriptions contain the word `chemistry`.

Therefore:

<div class="module-discovery-note">

  <span class="roary-page-eyebrow">
    SOFTWARE DISCOVERY
  </span>

  <h3>
    Use module avail first
  </h3>

  <p>
    To find whether a specific application is installed, use
    <code>module avail APPLICATION</code>.
  </p>

  <p>
    Use <code>module search STRING</code> only when searching module
    descriptions or keywords.
  </p>

</div>


## Module Descriptions

If a module provides `whatis` information, display it with:

```bash
module whatis NAME
```

For example:

```bash
module whatis gcc
```

The command displays descriptive information defined by the modulefile. :contentReference[oaicite:6]{index=6}


## Loading Software

After finding the exact module name:

```bash
module load NAME/VERSION
```

Example pattern:

```bash
module load gcc/11.5.0
```

Then verify that it loaded:

```bash
module list
```

Check the executable:

```bash
which gcc
```

Check the version:

```bash
gcc --version
```


### Use Explicit Versions

When possible, use:

```bash
module load NAME/VERSION
```

rather than relying on:

```bash
module load NAME
```

Explicit versions make research workflows easier to reproduce.


<div class="module-repro-panel">

  <div class="module-repro-mark">
    ✓
  </div>

  <div>

    <span class="roary-page-eyebrow">
      REPRODUCIBILITY
    </span>

    <h3>
      Record exact software versions
    </h3>

    <p>
      Keep the module name and version used by your workflow in your
      Slurm script, README, or project documentation.
    </p>

  </div>

</div>


## Show Loaded Modules

Run:

```bash
module list
```

This displays the modulefiles loaded in your current shell.

Always check this when troubleshooting a software environment.


## Inspect a Module

Before or after loading a module:

```bash
module show NAME/VERSION
```

You may also use:

```bash
module display NAME/VERSION
```

This shows what the modulefile changes in your environment.

Typical changes may include:

```text
PATH
LD_LIBRARY_PATH
MANPATH
CPATH
PKG_CONFIG_PATH
```

It may also show:

- Application paths
- Dependencies
- Conflicts
- Environment variables
- License variables
- Compiler or library requirements


## Verify the Executable

After loading software:

```bash
which PROGRAM
```

For example:

```bash
which python
```

or:

```bash
which gcc
```

You can also use:

```bash
command -v PROGRAM
```

Then check the software version:

```bash
PROGRAM --version
```


### Recommended Verification Pattern

```bash
module purge
module load SOFTWARE/VERSION
module list
which PROGRAM
PROGRAM --version
```


## Unload Software

Remove a module from the current shell:

```bash
module unload NAME
```

Example:

```bash
module unload gcc
```

Environment Modules supports dynamic loading and unloading of software
without installing or deleting the application itself. :contentReference[oaicite:7]{index=7}


## Switch Software Versions

If one version is loaded and you need another:

```bash
module switch OLD_MODULE NEW_MODULE
```

Example:

```bash
module switch gcc/OLD_VERSION gcc/NEW_VERSION
```

The official Environment Modules documentation demonstrates this
version-switching workflow. :contentReference[oaicite:8]{index=8}


## Start With a Clean Environment

Unload all currently loaded modules:

```bash
module purge
```

Then load only the modules required for your workflow:

```bash
module load SOFTWARE_A/VERSION
module load SOFTWARE_B/VERSION
```

This is useful when testing software or resolving dependency conflicts.


<div class="module-clean-panel">

  <span class="roary-page-eyebrow">
    TROUBLESHOOTING
  </span>

  <h3>
    Purge and rebuild the environment
  </h3>

  <p>
    If an application unexpectedly finds the wrong executable,
    compiler, MPI library, CUDA library, or dependency, start with
    <code>module purge</code> and load only the required modules.
  </p>

</div>


## Module Dependencies

Scientific applications often depend on a software stack.

A common pattern is:

<div class="module-dependency-flow">

  <div>
    <span>01</span>
    <strong>Compiler</strong>
    <small>GCC / Intel</small>
  </div>

  <div>→</div>

  <div>
    <span>02</span>
    <strong>MPI</strong>
    <small>OpenMPI / MPICH</small>
  </div>

  <div>→</div>

  <div>
    <span>03</span>
    <strong>Libraries</strong>
    <small>HDF5 / NetCDF / BLAS</small>
  </div>

  <div>→</div>

  <div>
    <span>04</span>
    <strong>Application</strong>
    <small>Research Software</small>
  </div>

</div>

Some modules load dependencies automatically.

Other modules may require prerequisite modules.

If loading a module prints instructions or an error, read the message
before continuing.


## Module Conflicts

Some software environments cannot safely coexist.

Examples include:

- Two compiler versions
- Multiple MPI implementations
- Incompatible CUDA toolkits
- Multiple versions of the same application
- Conflicting scientific libraries

If the module system reports a conflict:

```bash
module list
```

Then either unload the conflicting module:

```bash
module unload NAME
```

or start clean:

```bash
module purge
```


## Modules in Slurm Jobs

This is the most important operational rule.

<div class="module-job-rule">

  <div class="module-job-rule-mark">
    !
  </div>

  <div>

    <span class="roary-page-eyebrow">
      SLURM
    </span>

    <h3>
      Load required software inside the job script
    </h3>

    <p>
      Your Slurm script should explicitly create the software
      environment required by the workload.
    </p>

  </div>

</div>


### Recommended Job Pattern

```bash
#!/bin/bash

#SBATCH --job-name=module-example
#SBATCH --output=module_%j.out
#SBATCH --error=module_%j.err
#SBATCH --cpus-per-task=4
#SBATCH --mem=8G
#SBATCH --time=01:00:00

module purge
module load SOFTWARE/VERSION

echo "=== Loaded Modules ==="
module list

echo "=== Executable ==="
which PROGRAM

echo "=== Software Version ==="
PROGRAM --version

PROGRAM input.dat
```


### Why Put Modules in the Job Script?

It makes the job:

- Reproducible
- Easier to troubleshoot
- Independent of a previous login shell
- Clear about software versions
- Easier for another researcher to reproduce


## Record the Software Environment

For important jobs, include:

```bash
echo "=== Loaded Modules ==="
module list
```

You may also record:

```bash
echo "=== Host ==="
hostname
```

```bash
echo "=== Job ID ==="
echo "$SLURM_JOB_ID"
```

```bash
echo "=== Date ==="
date
```

This information can help when troubleshooting a job later.


## Modules in Open OnDemand

Interactive applications may also use environment modules.

Examples include:

- Jupyter
- RStudio
- VS Code
- MATLAB
- Interactive desktops
- Other research applications

The Open OnDemand application configuration may load the required
software automatically.

Open OnDemand compute sessions are still scheduled through Slurm.

See:

[Open OnDemand :material-arrow-right:](../../open-ondemand/){ .md-button .md-button--primary }


## Optional: Save a Module Collection

Environment Modules 5.5 supports saving and restoring collections of
loaded modules. :contentReference[oaicite:9]{index=9}

Configure your environment:

```bash
module purge
module load SOFTWARE_A/VERSION
module load SOFTWARE_B/VERSION
```

Save it:

```bash
module save myproject
```

List saved collections:

```bash
module savelist
```

Restore:

```bash
module restore myproject
```

Inspect:

```bash
module saveshow myproject
```

Delete:

```bash
module saverm myproject
```

Collections are convenient for interactive work, but Slurm job scripts
should still explicitly document required modules when reproducibility
matters.


## Module Search Path

View the directories searched for modulefiles:

```bash
echo $MODULEPATH
```

Advanced users can temporarily add another modulefile directory:

```bash
module use /path/to/modulefiles
```

Remove it:

```bash
module unuse /path/to/modulefiles
```

Do not modify Roary's centrally managed module directories.


## Common Problems

### Module Not Found

Example:

```text
ERROR: Unable to locate a modulefile
```

Check:

```bash
module avail NAME
```

Make sure the exact module name and version are correct.


### Command Not Found After Loading

Check:

```bash
module list
```

Then:

```bash
module show NAME/VERSION
```

and:

```bash
which PROGRAM
```


### Wrong Software Version

Run:

```bash
module list
```

```bash
which PROGRAM
```

```bash
PROGRAM --version
```

If necessary:

```bash
module purge
module load NAME/VERSION
```


### Works Interactively but Fails in Slurm

Make sure the Slurm script contains:

```bash
module purge
module load NAME/VERSION
```

Do not depend on modules loaded manually before running `sbatch`.


### Shared Library Errors

Errors mentioning:

```text
error while loading shared libraries
```

may indicate:

- Missing dependencies
- Wrong compiler environment
- Wrong MPI environment
- Incompatible library versions
- Incorrect software stack

Start with:

```bash
module list
```

Then test from a clean environment:

```bash
module purge
```


## Command Reference

| Task | Command |
|---|---|
| Check Modules version | `module --version` |
| List available modules | `module avail` |
| Find an application | `module avail NAME` |
| Case-insensitive lookup | `module -i avail NAME` |
| Highest available version | `module -L avail NAME` |
| Search module descriptions | `module search STRING` |
| Show module description | `module whatis NAME` |
| Inspect modulefile | `module show NAME/VERSION` |
| Load software | `module load NAME/VERSION` |
| Show loaded modules | `module list` |
| Unload module | `module unload NAME` |
| Switch versions | `module switch OLD NEW` |
| Unload all modules | `module purge` |
| Save collection | `module save NAME` |
| Restore collection | `module restore NAME` |
| List collections | `module savelist` |
| Show module search paths | `echo $MODULEPATH` |


## Best Practices

<div class="module-best-grid">

  <div>
    <span>01</span>
    <strong>Find software with module avail</strong>
    <p>Use the exact module name provided by Roary.</p>
  </div>

  <div>
    <span>02</span>
    <strong>Use explicit versions</strong>
    <p>Record versions for reproducible research.</p>
  </div>

  <div>
    <span>03</span>
    <strong>Verify the executable</strong>
    <p>Use which and --version before large jobs.</p>
  </div>

  <div>
    <span>04</span>
    <strong>Start clean when needed</strong>
    <p>Use module purge to remove conflicting environments.</p>
  </div>

  <div>
    <span>05</span>
    <strong>Load modules inside Slurm</strong>
    <p>Make the job define its own software environment.</p>
  </div>

  <div>
    <span>06</span>
    <strong>Record module list</strong>
    <p>Keep software information with job output.</p>
  </div>

</div>


<div class="module-golden-rule">

  <div class="module-golden-mark">
    !
  </div>

  <div>

    <span class="roary-page-eyebrow">
      GOLDEN RULE
    </span>

    <h3>
      Find with module avail, load explicitly, and record the version
    </h3>

    <p>
      Every reproducible Slurm workflow should clearly identify
      the software environment used by the computation.
    </p>

  </div>

</div>


## Need Help?

If the software you need does not appear in:

```bash
module avail NAME
```

email the HPC Admins:

[**hpcadmin@fiu.edu**](mailto:hpcadmin@fiu.edu)

Include:

- Your HPC username
- Research group or PI
- Software name
- Required version
- Official software page
- Whether the software requires a license
- Compiler, MPI, or CUDA requirements if known


## Next Software Guide

[Python :material-arrow-right:](../languages/python.md){ .md-button .md-button--primary }
