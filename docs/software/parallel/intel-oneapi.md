# Intel oneAPI

<div class="oneapi-hero">

  <div class="oneapi-hero-content">

    <span class="roary-page-eyebrow">
      COMPILERS
    </span>

    <h2>Intel oneAPI Compilers on Roary</h2>

    <p>
      Intel oneAPI provides optimized C, C++, and Fortran compilers commonly
      used for scientific and high-performance computing workloads.
    </p>

    <p>
      Roary provides Intel oneAPI through Environment Modules. Always check
      the available versions before compiling software.
    </p>

  </div>

  <div class="oneapi-hero-badge">
    <span>ROARY</span>
    <strong>oneAPI</strong>
    <small>INTEL</small>
  </div>

</div>


## Quick Start

Check available Intel oneAPI compiler versions:

```bash
module avail intel-oneapi
```

If needed, search more broadly:

```bash
module avail intel
```

Load an available version:

```bash
module load intel-oneapi-compilers/VERSION
```

Check the compilers:

```bash
which icx
which icpx
which ifx
```

Verify:

```bash
icx --version
icpx --version
ifx --version
```

!!! tip "Use the live module list"
    Module names and versions may change.

    Always use:

    ```bash
    module avail intel
    ```

    before loading Intel oneAPI.


## Intel oneAPI Compilers

Modern Intel oneAPI provides:

| Language | Compiler |
|---|---|
| C | `icx` |
| C++ | `icpx` |
| Fortran | `ifx` |

Older Intel compiler names such as:

```text
icc
icpc
ifort
```

may appear in older software documentation, but new workflows should normally
use the modern oneAPI compilers when available.


## Compile a C Program

Example source:

```c
#include <stdio.h>

int main(void) {
    printf("Hello from Intel oneAPI on Roary!\n");
    return 0;
}
```

Save as:

```text
hello.c
```

Compile:

```bash
icx hello.c -o hello
```

Run:

```bash
./hello
```


## Compile a C++ Program

Example:

```cpp
#include <iostream>

int main() {
    std::cout << "Hello from Intel oneAPI on Roary!" << std::endl;
    return 0;
}
```

Save as:

```text
hello.cpp
```

Compile:

```bash
icpx hello.cpp -o hello
```

Run:

```bash
./hello
```


## Compile a Fortran Program

Example:

```fortran
program hello
    print *, "Hello from Intel oneAPI on Roary!"
end program hello
```

Save as:

```text
hello.f90
```

Compile:

```bash
ifx hello.f90 -o hello
```

Run:

```bash
./hello
```


## Compiler Optimization

A useful starting point for optimized builds is:

```bash
icx -O2 program.c -o program
```

For C++:

```bash
icpx -O2 program.cpp -o program
```

For Fortran:

```bash
ifx -O2 program.f90 -o program
```

Common options include:

| Option | Purpose |
|---|---|
| `-O0` | Disable optimization |
| `-O2` | General optimization |
| `-O3` | More aggressive optimization |
| `-g` | Include debugging information |
| `-qopenmp` | Enable OpenMP |
| `-Wall` | Enable common warnings for C/C++ |

For development:

```bash
icx -O2 -Wall program.c -o program
```

!!! note
    More aggressive optimization does not always improve performance.

    Benchmark important applications before choosing compiler options.


## OpenMP with Intel oneAPI

Intel oneAPI supports OpenMP.

Compile a C OpenMP program:

```bash
icx -O2 -qopenmp program.c -o program
```

C++:

```bash
icpx -O2 -qopenmp program.cpp -o program
```

Fortran:

```bash
ifx -O2 -qopenmp program.f90 -o program
```

For a Slurm job requesting:

```bash
#SBATCH --cpus-per-task=8
```

set:

```bash
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
```

This keeps the number of OpenMP threads aligned with the CPUs allocated by
Slurm.

[OpenMP :material-arrow-right:](openmp.md){ .md-button }


## Intel oneMKL

Intel oneAPI also provides highly optimized mathematical libraries through
**oneMKL**.

oneMKL includes optimized routines for areas such as:

- BLAS
- LAPACK
- Linear algebra
- FFTs
- Vector mathematics

Check available Intel modules:

```bash
module avail intel
```

or:

```bash
module avail mkl
```

If oneMKL is available through the loaded Intel environment, Intel compilers
can commonly link against it using:

```bash
icx -O2 -qmkl program.c -o program
```

For C++:

```bash
icpx -O2 -qmkl program.cpp -o program
```

For Fortran:

```bash
ifx -O2 -qmkl program.f90 -o program
```

!!! important
    Library availability and module dependencies can vary by installed oneAPI
    version.

    Check the module environment before building:

    ```bash
    module list
    module show intel-oneapi-compilers/VERSION
    ```


## Intel oneAPI and MPI

MPI applications must use a compatible compiler and MPI software stack.

A typical software stack may look like:

```text
Intel oneAPI Compiler
        ↓
Compatible MPI
        ↓
MPI Application
```

If OpenMPI is being used, first check which OpenMPI modules are available:

```bash
module avail openmpi
```

Then load a compatible compiler/MPI combination.

For example:

```bash
module purge

module load intel-oneapi-compilers/VERSION
module load openmpi/VERSION
```

Check the MPI compiler wrapper:

```bash
which mpicc
which mpicxx
which mpifort
```

MPI applications should normally be compiled using the MPI wrappers:

```bash
mpicc program.c -o program
```

```bash
mpicxx program.cpp -o program
```

```bash
mpifort program.f90 -o program
```

!!! warning "Do not mix incompatible compiler and MPI stacks"
    An MPI application compiled with one compiler/MPI combination may fail
    when run with a different MPI implementation or compiler runtime.

[OpenMPI :material-arrow-right:](openmpi.md){ .md-button .md-button--primary }


## GCC vs Intel oneAPI

Roary may provide both GCC and Intel oneAPI.

| GCC | Intel oneAPI |
|---|---|
| `gcc` | `icx` |
| `g++` | `icpx` |
| `gfortran` | `ifx` |
| `-fopenmp` | `-qopenmp` |

Both are suitable for HPC applications.

Use the compiler required by your application or dependency stack.

Do not switch compiler families in the middle of a software build unless the
application explicitly supports it.

[GCC :material-arrow-right:](gcc.md){ .md-button }


## Running Intel-Compiled Programs with Slurm

Compilation and execution are separate steps.

After compiling:

```bash
icx -O2 analysis.c -o analysis
```

run computational workloads through Slurm.

Example:

```bash
#!/bin/bash

#SBATCH --job-name=oneapi-example
#SBATCH --output=oneapi_%j.out
#SBATCH --error=oneapi_%j.err
#SBATCH --cpus-per-task=1
#SBATCH --mem=4G
#SBATCH --time=00:30:00

module purge
module load intel-oneapi-compilers/VERSION

echo "======================================"
echo "Job ID       : $SLURM_JOB_ID"
echo "Node         : $(hostname)"
echo "Compiler     : $(icx --version | head -1)"
echo "======================================"

./analysis
```

Submit:

```bash
sbatch job.sh
```

!!! important
    Load the same compatible compiler environment at runtime that was used
    when building software that depends on Intel runtime libraries.

[Running Jobs :material-arrow-right:](../../running-jobs/index.md){ .md-button .md-button--primary }


## OpenMP Slurm Example

For an OpenMP application:

```bash
#!/bin/bash

#SBATCH --job-name=oneapi-openmp
#SBATCH --output=oneapi_openmp_%j.out
#SBATCH --error=oneapi_openmp_%j.err
#SBATCH --cpus-per-task=8
#SBATCH --mem=8G
#SBATCH --time=01:00:00

module purge
module load intel-oneapi-compilers/VERSION

export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK

echo "Node: $HOSTNAME"
echo "OpenMP threads: $OMP_NUM_THREADS"

./my_openmp_program
```

Submit:

```bash
sbatch job.sh
```


## Reproducible Builds

Record the compiler and module versions used to build important software.

Check:

```bash
module list
```

and:

```bash
icx --version
```

For Fortran:

```bash
ifx --version
```

A reproducible build script should explicitly load the required compiler:

```bash
module purge
module load intel-oneapi-compilers/VERSION

icx -O2 program.c -o program
```


## Common Problems

### `icx: command not found`

Check Intel modules:

```bash
module avail intel
```

Load the compiler:

```bash
module load intel-oneapi-compilers/VERSION
```

Verify:

```bash
which icx
icx --version
```


### `ifx: command not found`

Check:

```bash
module list
module avail intel
```

Then verify:

```bash
which ifx
ifx --version
```


### Wrong Compiler Version

Check:

```bash
module list
which icx
icx --version
```

Start with a clean environment if necessary:

```bash
module purge
module load intel-oneapi-compilers/VERSION
```


### Missing Shared Library

Example:

```text
error while loading shared libraries
```

Check the application:

```bash
ldd ./program
```

Then check:

```bash
module list
```

The application may require the same Intel runtime environment that was loaded
when it was compiled.


### MPI Program Fails

Check the complete software stack:

```bash
module list
which mpicc
mpicc --showme
```

Also check:

```bash
which icx
icx --version
```

Make sure the compiler and MPI implementation are compatible.


## Best Practices

- Use `module avail intel` to find available oneAPI versions.
- Use `icx` for C.
- Use `icpx` for C++.
- Use `ifx` for Fortran.
- Use explicit module versions for important builds.
- Use `module purge` when creating a clean compiler environment.
- Use `-O2` as a reasonable starting point for optimized builds.
- Use `-qopenmp` for OpenMP applications.
- Match `OMP_NUM_THREADS` to `SLURM_CPUS_PER_TASK`.
- Do not mix incompatible GCC, Intel, or MPI software stacks.
- Record compiler versions for reproducible research.
- Run computational workloads through Slurm.


## Need Help?

Collect:

```bash
hostname
module list
module avail intel
```

For C:

```bash
which icx
icx --version
```

For C++:

```bash
which icpx
icpx --version
```

For Fortran:

```bash
which ifx
ifx --version
```

If compilation fails, include the complete compiler command and error output.

For assistance:

[**hpcadmin@fiu.edu**](mailto:hpcadmin@fiu.edu)


## Related Guides

[Environment Modules](../modules/index.md){ .md-button }

[GCC](gcc.md){ .md-button }

[OpenMPI](openmpi.md){ .md-button }

[OpenMP](openmp.md){ .md-button }

[Running Jobs](../../running-jobs/index.md){ .md-button }
