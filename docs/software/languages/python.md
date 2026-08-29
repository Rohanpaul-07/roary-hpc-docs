# Python

<div class="app-hero">
  <div class="app-hero-content">

    <span class="roary-page-eyebrow">LANGUAGES & ENVIRONMENTS</span>

    <h2>Python on Roary</h2>

    <p>
      Python is widely used on Roary for scientific computing,
      data analysis, bioinformatics, automation, visualization,
      machine learning, and research workflows.
    </p>

    <p>
      Use project-specific Python environments for your research
      instead of installing packages into the operating-system Python.
    </p>

  </div>

  <div class="app-hero-badge">
    <span>ROARY</span>
    <strong>PYTHON</strong>
    <small>HPC</small>
  </div>
</div>


## Quick Start

Find Python installations available through the Roary module system:

```bash
module avail python
```

Check whether Miniconda is available:

```bash
module avail miniconda
```

Check the Python already available in your current environment:

```bash
which python
which python3
```

Check versions:

```bash
python --version
python3 --version
```

For a normal project, create a virtual environment:

```bash
mkdir -p ~/venvs
python3 -m venv ~/venvs/myproject
```

Activate it:

```bash
source ~/venvs/myproject/bin/activate
```

Then verify:

```bash
which python
python --version
```


## Python Environments on Roary

Users may encounter several different Python environments on Roary.

| Environment | Purpose |
|---|---|
| System Python | Python provided by AlmaLinux for operating-system tools |
| Module Python | Centrally managed Python versions, when provided through Environment Modules |
| Python `venv` | Lightweight project-specific Python environment |
| Conda / Miniconda | Python plus more complex compiled dependencies |
| Apptainer | Complete reproducible software/container environments |
| Jupyter | Browser-based interactive Python through Open OnDemand |


!!! warning "Do not modify the operating-system Python"

    The Python installation provided by AlmaLinux may be required by
    operating-system and administrative tools.

    Do not use `sudo pip`, `sudo python`, or try to install research
    packages into protected system Python directories.

    Use a virtual environment, Conda environment, Apptainer container,
    or centrally managed software module instead.


## Finding Python

Search the Roary module system:

```bash
module avail python
```

If Python modules are displayed, load the exact module name and version
shown by Roary.

Example pattern:

```bash
module load PYTHON_MODULE/VERSION
```

Then check:

```bash
module list
```

```bash
which python
```

```bash
python --version
```


!!! info "Always check the live module list"

    Software versions can change over time.

    Use `module avail python` to see the Python versions currently
    available on Roary rather than relying on an old version number
    from documentation.


## Which Python Am I Using?

Because multiple Python installations can exist, always verify the
interpreter before installing packages or submitting a large job.

Run:

```bash
which python
```

```bash
which python3
```

Display the exact executable from Python itself:

```bash
python3 -c 'import sys; print(sys.executable)'
```

Display detailed version information:

```bash
python3 -c 'import sys; print(sys.version)'
```


## `python` vs `python3`

Depending on your environment:

```bash
python
```

and:

```bash
python3
```

may refer to different executables.

Check both:

```bash
which python
which python3
```

```bash
python --version
python3 --version
```

Inside an activated virtual environment, `python` normally points to
the interpreter belonging to that environment.


## Choosing an Environment

Use this general guide:

| Need | Recommended method |
|---|---|
| Simple Python project | `venv` |
| Python packages installed with pip | `venv` |
| Complex scientific dependency stack | Conda / Miniconda |
| GPU framework with many dependencies | Conda or supported module/environment |
| Reproducible complete software stack | Apptainer |
| Centrally installed software | Environment Modules |
| Notebook workflow | Open OnDemand / Jupyter |


## Python Virtual Environments

A virtual environment isolates packages for one project.

This avoids dependency conflicts between unrelated research projects.


### Create an Environment Directory

```bash
mkdir -p ~/venvs
```


### Create an Environment

```bash
python3 -m venv ~/venvs/myproject
```


### Activate It

```bash
source ~/venvs/myproject/bin/activate
```

Your prompt may change to something similar to:

```text
(myproject) [username@login1 ~]$
```


### Verify It

```bash
which python
```

The path should point into your environment, for example:

```text
/home/USERNAME/venvs/myproject/bin/python
```

Also check:

```bash
python -c 'import sys; print(sys.executable)'
```

and:

```bash
python -c 'import sys; print(sys.prefix)'
```


### Check Whether a Virtual Environment Is Active

```bash
python -c 'import sys; print(sys.prefix != sys.base_prefix)'
```

If the result is:

```text
True
```

you are running inside a virtual environment.


### Deactivate the Environment

```bash
deactivate
```


## Where Should Python Environments Be Stored?

Persistent Python environments should be kept in persistent storage.

A simple organization is:

```text
/home/USERNAME/venvs/
```

For example:

```text
/home/USERNAME/venvs/genomics
/home/USERNAME/venvs/ml-project
/home/USERNAME/venvs/statistics
```

Do not keep an environment that must survive long-term in `/scratch`.

Roary scratch storage is temporary and subject to purge policies.


## pip

After activating your environment, upgrade pip:

```bash
python -m pip install --upgrade pip
```

Prefer:

```bash
python -m pip
```

instead of simply:

```bash
pip
```

Using `python -m pip` ensures that pip belongs to the Python interpreter
you are currently using.


### Install a Package

```bash
python -m pip install PACKAGE
```

Example:

```bash
python -m pip install numpy
```


### Install Several Packages

```bash
python -m pip install numpy scipy pandas matplotlib
```


### Install a Specific Version

```bash
python -m pip install PACKAGE==VERSION
```


### List Installed Packages

```bash
python -m pip list
```


### Inspect One Package

```bash
python -m pip show PACKAGE
```


### Check Dependency Problems

```bash
python -m pip check
```


## Reproducible Python Environments

After configuring a project environment, record the package versions:

```bash
python -m pip freeze > requirements.txt
```

View the file:

```bash
cat requirements.txt
```

To recreate the environment later:

```bash
python3 -m venv ~/venvs/myproject-new
```

```bash
source ~/venvs/myproject-new/bin/activate
```

```bash
python -m pip install --upgrade pip
```

```bash
python -m pip install -r requirements.txt
```


!!! success "Recommended practice"

    Keep `requirements.txt` with your source code.

    This makes it much easier to reproduce the software environment
    used for a research workflow.


## Recommended Project Layout

A Python project might look like:

```text
myproject/
├── README.md
├── requirements.txt
├── scripts/
│   ├── preprocess.py
│   └── analysis.py
├── jobs/
│   └── analysis.slurm
├── input/
├── logs/
└── results/
```

Keep the Python environment separately:

```text
~/venvs/myproject
```


## Lightweight Python on Login Nodes

Commands such as these are fine for checking an environment:

```bash
python --version
```

```bash
python -c 'print("Hello from Roary")'
```

```bash
python -c 'import numpy; print(numpy.__version__)'
```

Editing scripts and performing very small tests are also appropriate.


!!! danger "Do not run research workloads on login nodes"

    CPU-intensive, memory-intensive, long-running, highly parallel,
    or GPU-enabled Python applications must run on compute nodes
    through Slurm.


## Simple Python Example

Create:

```text
hello.py
```

with:

```python
import socket
import sys

print("Hello from Roary")
print("Hostname:", socket.gethostname())
print("Python version:", sys.version)
print("Python executable:", sys.executable)
```

A lightweight test can be run with:

```bash
python hello.py
```


## Running Python with Slurm

Create a log directory:

```bash
mkdir -p logs
```

Create a Slurm job script named:

```text
python_job.slurm
```

with:

```bash
#!/bin/bash

#SBATCH --job-name=python-test
#SBATCH --output=logs/python_%j.out
#SBATCH --error=logs/python_%j.err
#SBATCH --cpus-per-task=1
#SBATCH --mem=2G
#SBATCH --time=00:30:00

module purge

source ~/venvs/myproject/bin/activate

echo "========================================"
echo "Job ID:       $SLURM_JOB_ID"
echo "Node:         $(hostname)"
echo "Start time:   $(date)"
echo "Python:       $(which python)"
echo "Python ver:   $(python --version 2>&1)"
echo "========================================"

python hello.py

echo "========================================"
echo "End time:     $(date)"
echo "========================================"
```

Submit the job:

```bash
sbatch python_job.slurm
```

Check it:

```bash
squeue -u $USER
```


## Virtual Environments Created from Module Python

If a virtual environment was created using a Python module, load the
same base module before activating the environment.

Example pattern:

```bash
module purge
module load PYTHON_MODULE/VERSION

source ~/venvs/myproject/bin/activate

python analysis.py
```

This is important because the virtual environment may depend on the
base Python installation from which it was created.


## CPU Requests

Requesting more CPUs does not automatically make Python faster.

For example:

```bash
#SBATCH --cpus-per-task=8
```

allocates eight CPUs.

However, a normal single-threaded Python program may still use only
one CPU.

Request only the number of CPUs your program can actually use.


## Python Multiprocessing

Python applications using the standard `multiprocessing` package can
use multiple CPUs.

The Slurm allocation can be read from:

```bash
echo "$SLURM_CPUS_PER_TASK"
```

Python can access it through:

```python
import os

cpus = int(os.environ.get("SLURM_CPUS_PER_TASK", "1"))

print("Allocated CPUs:", cpus)
```


## NumPy, SciPy, and Threaded Libraries

Scientific Python packages may use threaded numerical libraries.

Examples include:

- NumPy
- SciPy
- scikit-learn
- OpenBLAS
- Intel MKL
- OpenMP-based libraries

A useful Slurm configuration is:

```bash
export OMP_NUM_THREADS=${SLURM_CPUS_PER_TASK:-1}
export OPENBLAS_NUM_THREADS=${SLURM_CPUS_PER_TASK:-1}
export MKL_NUM_THREADS=${SLURM_CPUS_PER_TASK:-1}
```

Then run:

```bash
python analysis.py
```


!!! tip "Match threads to allocated CPUs"

    Do not allow a Python numerical library to create more worker
    threads than the CPUs allocated to the Slurm job.


## Memory Requests

Python can use significant memory when working with:

- NumPy arrays
- pandas DataFrames
- Large genomic datasets
- Images
- Machine-learning models
- Large dictionaries
- Scientific matrices
- Large in-memory datasets

Request memory with Slurm:

```bash
#SBATCH --mem=16G
```

The correct amount depends on the program and dataset.


## Check Memory Usage

After a job finishes:

```bash
sacct -j JOBID \
--format=JobID,JobName,State,Elapsed,AllocCPUS,ReqMem,MaxRSS
```

`MaxRSS` helps estimate how much memory the job actually used.


## Out-of-Memory Errors

Possible symptoms include:

```text
OUT_OF_MEMORY
```

or:

```text
oom-kill
```

or:

```text
Killed
```

Check:

```bash
sacct -j JOBID \
--format=JobID,State,Elapsed,ReqMem,MaxRSS
```

Then determine whether:

- More memory is required
- The application can process smaller chunks
- Data can be streamed instead of fully loaded into memory
- A high-memory resource is appropriate


## Python Output Buffering

Python may buffer output when running non-interactively.

If a Slurm `.out` file is not updating as expected, use:

```bash
python -u analysis.py
```

or:

```bash
export PYTHONUNBUFFERED=1
```

then:

```bash
python analysis.py
```


## Slurm Variables from Python

Python can read Slurm environment variables.

Example:

```python
import os

print("Job ID:", os.environ.get("SLURM_JOB_ID"))
print("CPUs:", os.environ.get("SLURM_CPUS_PER_TASK"))
print("Node:", os.environ.get("SLURMD_NODENAME"))
```


## Python Job Arrays

When one Python program must process many independent files or samples,
a Slurm job array may be appropriate.

Example:

```bash
#!/bin/bash

#SBATCH --job-name=python-array
#SBATCH --array=0-9
#SBATCH --output=logs/python_%A_%a.out
#SBATCH --error=logs/python_%A_%a.err
#SBATCH --cpus-per-task=1
#SBATCH --mem=4G
#SBATCH --time=01:00:00

module purge

source ~/venvs/myproject/bin/activate

python analyze_sample.py "$SLURM_ARRAY_TASK_ID"
```

Each array task receives a different value through:

```text
SLURM_ARRAY_TASK_ID
```


## Packages That Require Compilation

Some Python packages contain C, C++, or Fortran components.

Installation output may reference:

```text
gcc
g++
gfortran
cmake
make
```

If compilation is required:

1. Check whether pip provides a prebuilt wheel.
2. Consider Conda for complex scientific dependencies.
3. Check available compilers:

```bash
module avail gcc
```

4. Do not run extremely large compilations on shared login nodes.
5. Email the HPC Admins if system libraries or centrally managed
   dependencies are required.


## pip Cache

Show the pip cache location:

```bash
python -m pip cache dir
```

Show cache usage:

```bash
python -m pip cache info
```

Remove unnecessary cached packages:

```bash
python -m pip cache purge
```

Python environments and caches consume storage.

Check your home quota with:

```bash
myquota
```


## Jupyter

For notebook-based Python workflows, use Roary Open OnDemand:

```text
https://hpclogin.fiu.edu
```

Jupyter sessions launched through Open OnDemand can request compute
resources through Slurm.

See:

[Jupyter :material-arrow-right:](../interactive/jupyter.md){ .md-button .md-button--primary }


## Python and GPUs

GPU-enabled Python applications may include:

- PyTorch
- TensorFlow
- JAX
- CuPy
- Other CUDA-enabled libraries

GPU workloads must run inside a GPU allocation obtained through Slurm.

See:

[GPU Computing :material-arrow-right:](../../gpus/index.md){ .md-button .md-button--primary }

[PyTorch :material-arrow-right:](../ai-ml/pytorch.md){ .md-button }

[TensorFlow :material-arrow-right:](../ai-ml/tensorflow.md){ .md-button }


## MPI Python

Distributed Python programs may use:

```text
mpi4py
```

MPI Python must use an MPI implementation compatible with the
environment in which `mpi4py` was built.

Do not mix unrelated MPI implementations.

See:

[OpenMPI :material-arrow-right:](../parallel/openmpi.md){ .md-button }


## Useful Diagnostics

### Python executable

```bash
python -c 'import sys; print(sys.executable)'
```


### Python version

```bash
python -c 'import sys; print(sys.version)'
```


### Python search path

```bash
python -c 'import sys; print("\n".join(sys.path))'
```


### Environment prefix

```bash
python -c 'import sys; print(sys.prefix)'
```


### Python site configuration

```bash
python -m site
```


### pip location

```bash
python -m pip --version
```


### Installed packages

```bash
python -m pip list
```


### Package information

```bash
python -m pip show PACKAGE
```


## Test Common Scientific Packages

Test NumPy:

```bash
python -c 'import numpy; print(numpy.__version__)'
```

Test pandas:

```bash
python -c 'import pandas; print(pandas.__version__)'
```

Test SciPy:

```bash
python -c 'import scipy; print(scipy.__version__)'
```


## Common Problems

### `ModuleNotFoundError`

Example:

```text
ModuleNotFoundError: No module named 'pandas'
```

Check your Python:

```bash
which python
```

Check the package:

```bash
python -m pip show pandas
```

If the package is missing from the current environment:

```bash
python -m pip install pandas
```


### Package Is Installed but Cannot Be Imported

This often means Python and pip belong to different environments.

Check:

```bash
which python
```

```bash
python -m pip --version
```

```bash
python -c 'import sys; print(sys.executable)'
```

Install using:

```bash
python -m pip install PACKAGE
```


### Permission Denied During Installation

Do not install into system Python locations.

Create a virtual environment:

```bash
python3 -m venv ~/venvs/myproject
```

Activate it:

```bash
source ~/venvs/myproject/bin/activate
```

Then install:

```bash
python -m pip install PACKAGE
```


### Shared-Library Errors

Example:

```text
ImportError: libXYZ.so: cannot open shared object file
```

Possible causes include:

- Missing software dependency
- Wrong compiler environment
- Incompatible library
- Package built against another Python
- Package built against another MPI implementation
- Mixing incompatible Conda and module libraries

Check:

```bash
module list
```

```bash
which python
```

```bash
python -m pip show PACKAGE
```


### Works Interactively but Fails in Slurm

Make sure the Slurm job recreates the environment:

```bash
module purge

source ~/venvs/myproject/bin/activate

which python
python --version

python analysis.py
```


### Job Was Killed

Check accounting:

```bash
sacct -j JOBID \
--format=JobID,State,Elapsed,AllocCPUS,ReqMem,MaxRSS
```

The job may have exceeded its requested memory.


### No Space Left on Device

Check your quota:

```bash
myquota
```

Check Python environment sizes:

```bash
du -sh ~/venvs/*
```

Check pip cache usage:

```bash
python -m pip cache info
```


## What Not to Do

- Do not use `sudo pip`.
- Do not modify the operating-system Python.
- Do not run computational Python jobs on login nodes.
- Do not assume `pip` belongs to the same interpreter as `python`.
- Do not request dozens of CPUs for single-threaded code.
- Do not let threaded libraries exceed your Slurm CPU allocation.
- Do not store permanent environments in `/scratch`.
- Do not mix unrelated compiler, MPI, CUDA, Conda, and module environments without understanding the dependencies.


## Recommended Python Workflow

1. Check Python with `module avail python`.
2. Select the appropriate Python environment.
3. Create one environment per project.
4. Activate the environment.
5. Install packages with `python -m pip`.
6. Record dependencies in `requirements.txt`.
7. Test the environment with a lightweight command.
8. Create a Slurm job script.
9. Request realistic CPU and memory resources.
10. Run the computation on a compute node.
11. Review output and resource usage.


## Command Reference

| Task | Command |
|---|---|
| Find Python modules | `module avail python` |
| Find Miniconda | `module avail miniconda` |
| Find Python executable | `which python` |
| Check Python version | `python --version` |
| Create environment | `python3 -m venv ~/venvs/NAME` |
| Activate environment | `source ~/venvs/NAME/bin/activate` |
| Deactivate environment | `deactivate` |
| Upgrade pip | `python -m pip install --upgrade pip` |
| Install package | `python -m pip install PACKAGE` |
| Install exact version | `python -m pip install PACKAGE==VERSION` |
| List packages | `python -m pip list` |
| Show package details | `python -m pip show PACKAGE` |
| Check dependencies | `python -m pip check` |
| Save environment | `python -m pip freeze > requirements.txt` |
| Restore environment | `python -m pip install -r requirements.txt` |
| Show pip cache | `python -m pip cache info` |
| Clear pip cache | `python -m pip cache purge` |


## Best Practices

- Use one environment per research project.
- Prefer `python -m pip` over a standalone `pip` command.
- Record exact package versions.
- Verify `which python` before installing packages.
- Verify Python again inside Slurm jobs.
- Run heavy workloads through Slurm.
- Match CPU/thread counts to the Slurm allocation.
- Review `MaxRSS` after memory-intensive jobs.
- Keep important environments in persistent storage.
- Use Conda or Apptainer when `venv` is not sufficient.


!!! important "Golden Rule"

    Before launching an expensive Python computation, know exactly:

    - Which Python executable is being used
    - Which Python version is being used
    - Which environment is active
    - Which package versions are installed
    - How many CPUs the code can use
    - How much memory the job requires
    - Whether GPU resources are required


## Need Help?

For Python package installation, compiled dependencies, environment
problems, or Python jobs that behave differently under Slurm, email:

[**hpcadmin@fiu.edu**](mailto:hpcadmin@fiu.edu)

Include:

- HPC username
- Research group or PI
- Python version
- Environment type
- Package name and version
- Exact command used
- Complete error message
- Slurm job ID, if applicable


## Next Guide

[Conda / Miniconda :material-arrow-right:](conda.md){ .md-button .md-button--primary }
