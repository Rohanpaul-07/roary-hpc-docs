# GCC Compiler

<div class="gcc-hero">

  <div class="gcc-hero-content">

    <span class="roary-page-eyebrow">
      COMPILERS
    </span>

    <h2>GNU Compiler Collection on Roary</h2>

    <p>
      GCC provides the standard GNU compilers used to build C, C++,
      Fortran, OpenMP, MPI, and many scientific applications on Roary.
    </p>

    <p>
      Multiple GCC versions may be available through Environment Modules.
      Always check the currently installed versions before compiling software.
    </p>

  </div>

  <div class="gcc-hero-badge">
    <span>ROARY</span>
    <strong>GCC</strong>
    <small>COMPILER</small>
  </div>

</div>


## Quick Start

Check available GCC versions:

```bash
module avail gcc
```

Load a version:

```bash
module load gcc/VERSION
```

Check the compilers:

```bash
which gcc
which g++
which gfortran
```

Check the version:

```bash
gcc --version
```

!!! tip
    Replace `VERSION` with an actual version shown by:

    ```bash
    module avail gcc
    ```


## GCC Compilers

GCC provides compilers for several commonly used HPC languages.

| Language | Compiler |
|---|---|
| C | `gcc` |
| C++ | `g++` |
| Fortran | `gfortran` |

Verify them with:

```bash
gcc --version
g++ --version
gfortran --version
```


## Compile a C Program

Example source:

```c
#include <stdio.h>

int main(void) {
    printf("Hello from Roary!\n");
    return 0;
}
```

Save it as:

```text
hello.c
```

Compile:

```bash
gcc hello.c -o hello
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
    std::cout << "Hello from Roary!" << std::endl;
    return 0;
}
```

Save as:

```text
hello.cpp
```

Compile:

```bash
g++ hello.cpp -o hello
```

Run:

```bash
./hello
```


## Compile a Fortran Program

Example:

```fortran
program hello
    print *, "Hello from Roary!"
end program hello
```

Save as:

```text
hello.f90
```

Compile:

```bash
gfortran hello.f90 -o hello
```

Run:

```bash
./hello
```


## Recommended Compiler Options

A common optimized build is:

```bash
gcc -O2 program.c -o program
```

For C++:

```bash
g++ -O2 program.cpp -o program
```

For Fortran:

```bash
gfortran -O2 program.f90 -o program
```

Useful options include:

| Option | Purpose |
|---|---|
| `-O0` | No optimization; useful for debugging |
| `-O2` | Recommended general optimization |
| `-O3` | More aggressive optimization |
| `-g` | Include debugging information |
| `-Wall` | Enable common compiler warnings |
| `-fopenmp` | Enable OpenMP support |

For development, a useful combination is:

```bash
gcc -O2 -Wall program.c -o program
```

!!! note
    Higher optimization such as `-O3` does not always make an application
    faster. Test performance before using aggressive compiler options.


## OpenMP with GCC

GCC includes OpenMP support.

Compile an OpenMP program with:

```bash
gcc -O2 -fopenmp program.c -o program
```

For C++:

```bash
g++ -O2 -fopenmp program.cpp -o program
```

For Fortran:

```bash
gfortran -O2 -fopenmp program.f90 -o program
```

When running OpenMP applications through Slurm, the number of OpenMP threads
should normally match the CPUs allocated to the job:

```bash
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
```

See:

[OpenMP :material-arrow-right:](openmp.md){ .md-button }


## GCC and OpenMPI

MPI installations are normally built with a particular compiler stack.

A typical environment may look like:

```text
GCC
 ↓
OpenMPI
 ↓
MPI Application
```

Load the compiler before the matching MPI module when required:

```bash
module purge

module load gcc/VERSION
module load openmpi/VERSION
```

Then check:

```bash
which mpicc
which mpicxx
which mpifort
```

MPI applications should normally be compiled using the MPI compiler wrappers:

```bash
mpicc program.c -o program
```

```bash
mpicxx program.cpp -o program
```

```bash
mpifort program.f90 -o program
```

!!! warning
    Do not compile an MPI application with one compiler/MPI stack and then
    run it with an unrelated MPI implementation.

[OpenMPI :material-arrow-right:](openmpi.md){ .md-button .md-button--primary }


## Compiling Software on Roary

For small programs and quick builds, compilation on a login node is generally
appropriate.

Examples:

```bash
gcc hello.c -o hello
```

or:

```bash
make
```

Large software builds can use significant CPU and memory.

!!! warning "Large builds belong on compute nodes"
    If a build takes a long time, uses many compiler processes, or consumes
    significant CPU or memory, use an interactive compute allocation rather
    than heavily loading a shared login node.


## Running Compiled Programs with Slurm

Compilation and execution are separate steps.

After compiling:

```bash
gcc -O2 analysis.c -o analysis
```

run computational workloads through Slurm.

Example:

```bash
#!/bin/bash

#SBATCH --job-name=gcc-example
#SBATCH --output=gcc_%j.out
#SBATCH --error=gcc_%j.err
#SBATCH --cpus-per-task=1
#SBATCH --mem=4G
#SBATCH --time=00:30:00

module purge
module load gcc/VERSION

echo "======================================"
echo "Job ID      : $SLURM_JOB_ID"
echo "Node        : $(hostname)"
echo "GCC         : $(gcc --version | head -1)"
echo "======================================"

./analysis
```

Submit:

```bash
sbatch job.sh
```

[Running Jobs :material-arrow-right:](../../running-jobs/index.md){ .md-button .md-button--primary }


## Reproducible Builds

Record the compiler version used to build important research software.

Check:

```bash
gcc --version
```

and:

```bash
module list
```

For example, a build script should explicitly load the required compiler:

```bash
module purge
module load gcc/VERSION

gcc -O2 program.c -o program
```

This makes it easier to reproduce the application later.


## Common Problems

### `gcc: command not found`

Check available GCC modules:

```bash
module avail gcc
```

Load one:

```bash
module load gcc/VERSION
```

Verify:

```bash
which gcc
gcc --version
```


### Wrong GCC Version

Check:

```bash
module list
which gcc
gcc --version
```

Start with a clean environment if necessary:

```bash
module purge
module load gcc/VERSION
```


### Missing Header File

Example:

```text
fatal error: example.h: No such file or directory
```

The required development library or software dependency may not be loaded.

Check:

```bash
module avail
```

and inspect required software modules.


### Missing Shared Library

Example:

```text
error while loading shared libraries
```

Check the binary:

```bash
ldd ./program
```

Also check:

```bash
module list
```

Make sure the runtime environment uses the same compatible compiler and
library stack used when the application was built.


### `GLIBCXX` Error

Example:

```text
GLIBCXX_x.x.x not found
```

This commonly indicates that the application is using a different
`libstdc++` version than the one it was compiled with.

Check:

```bash
module list
gcc --version
```

Then start clean and load the intended compiler:

```bash
module purge
module load gcc/VERSION
```


## Best Practices

- Use `module avail gcc` to find available versions.
- Use an explicit GCC version for important builds.
- Use `module purge` before creating a clean compiler environment.
- Use `-O2` as a reasonable starting point for optimized builds.
- Use `-Wall` while developing C and C++ applications.
- Use `-fopenmp` only when compiling OpenMP applications.
- Use MPI compiler wrappers such as `mpicc` for MPI applications.
- Do not mix incompatible compiler and MPI stacks.
- Run computational workloads through Slurm.
- Record compiler versions for reproducible research.


## Need Help?

Collect:

```bash
hostname
module list
which gcc
gcc --version
```

For C++:

```bash
which g++
g++ --version
```

For Fortran:

```bash
which gfortran
gfortran --version
```

If compilation fails, include the complete compiler command and error message.

For assistance:

[**hpcadmin@fiu.edu**](mailto:hpcadmin@fiu.edu)


## Related Guides

[Environment Modules](../modules/index.md){ .md-button }

[OpenMPI](openmpi.md){ .md-button }

[OpenMP](openmp.md){ .md-button }

[Intel oneAPI](intel-oneapi.md){ .md-button }

[Running Jobs](../../running-jobs/index.md){ .md-button }
