# OpenMP

<div class="openmp-hero">

  <div class="openmp-hero-content">

    <span class="roary-page-eyebrow">
      PARALLEL COMPUTING
    </span>

    <h2>OpenMP on Roary</h2>

    <p>
      OpenMP allows a program to use multiple CPU cores through shared-memory
      parallelism. It is commonly used with C, C++, and Fortran applications.
    </p>

    <p>
      On Roary, OpenMP threads should be matched to the CPU cores requested
      from Slurm using <code>--cpus-per-task</code>.
    </p>

  </div>

  <div class="openmp-hero-badge">
    <span>ROARY</span>
    <strong>OpenMP</strong>
    <small>THREADS</small>
  </div>

</div>


## Quick Start

Load a compiler:

```bash
module avail gcc
module load gcc/VERSION
```

Compile an OpenMP program:

```bash
gcc -O2 -fopenmp program.c -o program
```

Request CPU cores in Slurm:

```bash
#SBATCH --cpus-per-task=8
```

Set the number of OpenMP threads:

```bash
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
```

Run:

```bash
./program
```

!!! important
    OpenMP uses shared memory and normally runs within a **single compute node**.

    Use MPI when an application needs to distribute work across multiple nodes.


## What Is OpenMP?

OpenMP provides shared-memory parallelism.

A single program can create multiple worker threads:

```text
One Process
   |
   +-- Thread 1
   +-- Thread 2
   +-- Thread 3
   +-- Thread 4
```

All threads share the memory of the same process.

This makes OpenMP useful for applications that need multiple CPU cores on one
compute node.


## OpenMP vs MPI

OpenMP and MPI solve different HPC problems.

| OpenMP | MPI |
|---|---|
| Shared-memory parallelism | Distributed-memory parallelism |
| Uses threads | Uses processes / ranks |
| Normally one node | Can use multiple nodes |
| `OMP_NUM_THREADS` | `--ntasks` |
| `--cpus-per-task` | `--ntasks` / `--nodes` |

For example:

```text
OpenMP:
1 process × 8 threads

MPI:
8 processes × 1 CPU each
```

Some applications combine both approaches.

[OpenMPI :material-arrow-right:](openmpi.md){ .md-button }


## Compile with GCC

Load GCC:

```bash
module avail gcc
module load gcc/VERSION
```

Compile C:

```bash
gcc -O2 -fopenmp program.c -o program
```

Compile C++:

```bash
g++ -O2 -fopenmp program.cpp -o program
```

Compile Fortran:

```bash
gfortran -O2 -fopenmp program.f90 -o program
```

[GCC :material-arrow-right:](gcc.md){ .md-button }


## Compile with Intel oneAPI

Load Intel oneAPI:

```bash
module avail intel
module load intel-oneapi-compilers/VERSION
```

Compile C:

```bash
icx -O2 -qopenmp program.c -o program
```

Compile C++:

```bash
icpx -O2 -qopenmp program.cpp -o program
```

Compile Fortran:

```bash
ifx -O2 -qopenmp program.f90 -o program
```

[Intel oneAPI :material-arrow-right:](intel-oneapi.md){ .md-button }


## Simple OpenMP Example

Example C program:

```c
#include <omp.h>
#include <stdio.h>

int main(void)
{
    #pragma omp parallel
    {
        int thread = omp_get_thread_num();
        int total  = omp_get_num_threads();

        printf("Hello from thread %d of %d\n", thread, total);
    }

    return 0;
}
```

Save it as:

```text
hello_openmp.c
```

Compile:

```bash
gcc -O2 -fopenmp hello_openmp.c -o hello_openmp
```

The number of threads is controlled with:

```bash
export OMP_NUM_THREADS=4
```

Then run:

```bash
./hello_openmp
```


## OpenMP Resources in Slurm

For OpenMP workloads, the most important Slurm option is:

```bash
#SBATCH --cpus-per-task=NUMBER
```

For example:

```bash
#SBATCH --cpus-per-task=8
```

means one task receives 8 CPU cores.

Set:

```bash
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
```

This gives:

```text
1 Slurm task
      ↓
8 allocated CPUs
      ↓
8 OpenMP threads
```


## Recommended Slurm Job

Example using 8 OpenMP threads:

```bash
#!/bin/bash

#SBATCH --job-name=openmp-test
#SBATCH --output=openmp_%j.out
#SBATCH --error=openmp_%j.err

#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=8

#SBATCH --mem=8G
#SBATCH --time=01:00:00

module purge
module load gcc/VERSION

export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK

echo "======================================"
echo "Job ID          : $SLURM_JOB_ID"
echo "Node            : $(hostname)"
echo "CPUs per task   : $SLURM_CPUS_PER_TASK"
echo "OpenMP threads  : $OMP_NUM_THREADS"
echo "======================================"

./my_openmp_program
```

Submit:

```bash
sbatch openmp_job.sh
```

!!! important
    For a normal OpenMP-only application, use:

    ```bash
    --nodes=1
    --ntasks=1
    --cpus-per-task=N
    ```

    where `N` is the number of OpenMP threads required.

[Running Jobs :material-arrow-right:](../../running-jobs/index.md){ .md-button .md-button--primary }


## Thread Placement

OpenMP threads can be bound to CPU cores.

A useful starting configuration is:

```bash
export OMP_PLACES=cores
export OMP_PROC_BIND=close
```

Together with:

```bash
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
```

Example:

```bash
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
export OMP_PLACES=cores
export OMP_PROC_BIND=close
```

This can help keep threads associated with allocated CPU cores.

!!! note
    Thread placement can affect performance.

    Applications with different memory-access patterns may benefit from
    different binding strategies, so benchmark important workloads.


## OpenMP Environment Variables

Useful variables include:

| Variable | Purpose |
|---|---|
| `OMP_NUM_THREADS` | Number of OpenMP threads |
| `OMP_PLACES` | Defines thread placement locations |
| `OMP_PROC_BIND` | Controls thread binding |
| `OMP_DISPLAY_ENV` | Displays OpenMP runtime settings |

To display the OpenMP runtime environment:

```bash
export OMP_DISPLAY_ENV=TRUE
```

Then run the application.


## Do Not Oversubscribe CPUs

If the Slurm job requests:

```bash
#SBATCH --cpus-per-task=8
```

do not set:

```bash
export OMP_NUM_THREADS=32
```

That would create more OpenMP threads than allocated CPU cores.

Use:

```bash
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
```

!!! warning
    Oversubscribing CPUs usually reduces performance and can interfere with
    other workloads on the compute node.


## OpenMP Does Not Span Multiple Nodes

An OpenMP program normally shares memory within one machine.

This means a job like:

```bash
#SBATCH --nodes=2
#SBATCH --cpus-per-task=8
```

does not automatically make a normal OpenMP application use both nodes.

For multi-node parallelism, use an MPI application or a hybrid MPI/OpenMP
application.


## Hybrid MPI + OpenMP

Some applications combine MPI and OpenMP.

Example:

```bash
#SBATCH --nodes=2
#SBATCH --ntasks-per-node=4
#SBATCH --cpus-per-task=8
```

This means:

```text
2 nodes
×
4 MPI ranks per node
×
8 OpenMP threads per rank
```

Set:

```bash
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
```

Then launch through Slurm:

```bash
srun ./hybrid_program
```

Example:

```bash
#!/bin/bash

#SBATCH --job-name=hybrid
#SBATCH --output=hybrid_%j.out
#SBATCH --error=hybrid_%j.err

#SBATCH --nodes=2
#SBATCH --ntasks-per-node=4
#SBATCH --cpus-per-task=8

#SBATCH --mem=32G
#SBATCH --time=01:00:00

module purge
module load openmpi/VERSION

export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
export OMP_PLACES=cores
export OMP_PROC_BIND=close

srun ./hybrid_program
```

[OpenMPI :material-arrow-right:](openmpi.md){ .md-button .md-button--primary }


## Numerical Libraries

Some scientific libraries may create CPU threads automatically.

Examples include:

- OpenBLAS
- Intel MKL
- NumPy
- SciPy
- Some machine-learning libraries

For threaded workloads, you may also see variables such as:

```bash
export OPENBLAS_NUM_THREADS=$SLURM_CPUS_PER_TASK
export MKL_NUM_THREADS=$SLURM_CPUS_PER_TASK
```

Use these only when relevant to the libraries used by your application.


## Common Problems

### Program Uses Only One CPU

Make sure it was compiled with OpenMP support.

For GCC:

```bash
gcc -O2 -fopenmp program.c -o program
```

Check:

```bash
echo "$OMP_NUM_THREADS"
```

and verify the Slurm request includes:

```bash
#SBATCH --cpus-per-task=N
```


### Too Many Threads

Check:

```bash
echo "$OMP_NUM_THREADS"
echo "$SLURM_CPUS_PER_TASK"
```

These should normally match:

```bash
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
```


### Program Works Interactively but Not in Slurm

Load the required compiler inside the job:

```bash
module purge
module load gcc/VERSION
```

Set:

```bash
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
```

Do not depend on settings from your login shell.


### OpenMP Symbols Are Missing During Compilation

Errors mentioning functions such as:

```text
omp_get_thread_num
```

may mean OpenMP support was not enabled.

For GCC, compile with:

```bash
-fopenmp
```

For Intel oneAPI:

```bash
-qopenmp
```


### Performance Gets Worse with More Threads

More threads do not always mean better performance.

Possible reasons include:

- Memory bandwidth limits
- Thread synchronization
- Small workload size
- Poor thread placement
- Too many threads
- Application scaling limitations

Test several thread counts such as:

```text
1
2
4
8
16
```

and compare runtime.


## Best Practices

- Use one Slurm task for normal OpenMP-only jobs.
- Request CPUs with `--cpus-per-task`.
- Set `OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK`.
- Keep OpenMP-only jobs on one compute node.
- Compile GCC programs with `-fopenmp`.
- Compile Intel oneAPI programs with `-qopenmp`.
- Do not create more threads than allocated CPUs.
- Consider `OMP_PLACES=cores` and `OMP_PROC_BIND=close`.
- Benchmark different thread counts for important workloads.
- Use MPI when work must span multiple nodes.
- Use MPI + OpenMP only when the application supports hybrid parallelism.
- Run computational workloads through Slurm.


## Need Help?

Collect:

```bash
hostname
module list

echo "$SLURM_CPUS_PER_TASK"
echo "$OMP_NUM_THREADS"
echo "$OMP_PLACES"
echo "$OMP_PROC_BIND"
```

For GCC applications:

```bash
gcc --version
```

For Intel oneAPI:

```bash
icx --version
```

For Slurm problems, also provide:

- Job ID
- Job script
- Output file
- Error file
- Number of requested CPUs

For assistance:

[**hpcadmin@fiu.edu**](mailto:hpcadmin@fiu.edu)


## Related Guides

[Environment Modules](../modules/index.md){ .md-button }

[GCC](gcc.md){ .md-button }

[Intel oneAPI](intel-oneapi.md){ .md-button }

[OpenMPI](openmpi.md){ .md-button }

[Running Jobs](../../running-jobs/index.md){ .md-button }
