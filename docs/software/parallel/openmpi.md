# OpenMPI

<div class="openmpi-hero">

  <div class="openmpi-hero-content">

    <span class="roary-page-eyebrow">
      PARALLEL COMPUTING
    </span>

    <h2>OpenMPI on Roary</h2>

    <p>
      OpenMPI allows applications to run multiple cooperating processes
      across one or more compute nodes.
    </p>

    <p>
      On Roary, MPI jobs are scheduled through Slurm. OpenMPI should be
      loaded through Environment Modules and used with a compatible compiler
      stack.
    </p>

  </div>

  <div class="openmpi-hero-badge">
    <span>ROARY</span>
    <strong>MPI</strong>
    <small>OPENMPI</small>
  </div>

</div>


## Quick Start

Check available OpenMPI versions:

```bash
module avail openmpi
```

Load an available version:

```bash
module load openmpi/VERSION
```

Verify:

```bash
which mpicc
which mpirun
mpirun --version
```

!!! tip "Check the available versions first"
    Always use:

    ```bash
    module avail openmpi
    ```

    because available OpenMPI versions may change.


## What Is MPI?

MPI stands for **Message Passing Interface**.

It allows multiple processes to communicate while running:

```text
MPI Job
  |
  +-- Rank 0
  +-- Rank 1
  +-- Rank 2
  +-- Rank 3
```

MPI can run processes:

- On a single compute node
- Across multiple compute nodes
- Across many CPU cores
- As part of large distributed scientific applications

Each MPI process is commonly called a **rank**.


## MPI Compiler Commands

OpenMPI provides compiler wrappers.

| Language | Command |
|---|---|
| C | `mpicc` |
| C++ | `mpicxx` |
| Fortran | `mpifort` |

These wrappers automatically include the required MPI headers and libraries.

Check them with:

```bash
which mpicc
which mpicxx
which mpifort
```

See which underlying compiler is being used:

```bash
mpicc --showme
```


## Compile an MPI Program

Example C program:

```c
#include <mpi.h>
#include <stdio.h>

int main(int argc, char **argv)
{
    int rank, size;

    MPI_Init(&argc, &argv);

    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    printf("Hello from rank %d of %d\n", rank, size);

    MPI_Finalize();

    return 0;
}
```

Save it as:

```text
hello_mpi.c
```

Load OpenMPI:

```bash
module load openmpi/VERSION
```

Compile:

```bash
mpicc -O2 hello_mpi.c -o hello_mpi
```

Do **not** compile MPI programs with plain:

```bash
gcc hello_mpi.c
```

Use the MPI wrapper:

```bash
mpicc
```


## MPI Resources in Slurm

The most important Slurm options for MPI are:

| Slurm option | Meaning |
|---|---|
| `--nodes` | Number of compute nodes |
| `--ntasks` | Total number of MPI processes |
| `--ntasks-per-node` | MPI processes per node |
| `--cpus-per-task` | CPU cores available to each MPI process |
| `--mem` | Memory per node |

For a normal MPI-only application:

```text
1 Slurm task = 1 MPI rank
```

For example:

```bash
#SBATCH --nodes=2
#SBATCH --ntasks=16
#SBATCH --ntasks-per-node=8
```

means:

```text
Node 1: 8 MPI ranks
Node 2: 8 MPI ranks

Total: 16 MPI ranks
```


## Single-Node MPI Job

Example with 8 MPI ranks on one node:

```bash
#!/bin/bash

#SBATCH --job-name=mpi-test
#SBATCH --output=mpi_%j.out
#SBATCH --error=mpi_%j.err

#SBATCH --nodes=1
#SBATCH --ntasks=8
#SBATCH --time=00:30:00
#SBATCH --mem=8G

module purge
module load openmpi/VERSION

echo "======================================"
echo "Job ID     : $SLURM_JOB_ID"
echo "Nodes      : $SLURM_JOB_NUM_NODES"
echo "MPI tasks  : $SLURM_NTASKS"
echo "Node list  : $SLURM_NODELIST"
echo "======================================"

module list

srun ./hello_mpi
```

Submit:

```bash
sbatch mpi_job.sh
```


## Multi-Node MPI Job

Example using 2 nodes and 16 MPI ranks:

```bash
#!/bin/bash

#SBATCH --job-name=mpi-multinode
#SBATCH --output=mpi_%j.out
#SBATCH --error=mpi_%j.err

#SBATCH --nodes=2
#SBATCH --ntasks=16
#SBATCH --ntasks-per-node=8

#SBATCH --time=01:00:00
#SBATCH --mem=16G

module purge
module load openmpi/VERSION

echo "======================================"
echo "Job ID        : $SLURM_JOB_ID"
echo "Nodes         : $SLURM_JOB_NUM_NODES"
echo "MPI tasks     : $SLURM_NTASKS"
echo "Tasks / node  : $SLURM_NTASKS_PER_NODE"
echo "Node list     : $SLURM_NODELIST"
echo "======================================"

module list

srun ./my_mpi_program
```

Submit:

```bash
sbatch mpi_job.sh
```

!!! important
    Do not manually SSH into compute nodes to launch MPI processes.

    Slurm allocates the nodes and launches the parallel workload.


## `srun` vs `mpirun`

On a Slurm cluster, `srun` is the preferred starting point for launching
parallel jobs because Slurm already knows:

- Which nodes were allocated
- How many tasks were requested
- Where each task should run
- Which resources belong to the job

Example:

```bash
srun ./my_mpi_program
```

Some OpenMPI applications may also support:

```bash
mpirun ./my_mpi_program
```

inside a Slurm allocation.

For normal Roary jobs, start with the Slurm-integrated launch method shown in
this guide:

```bash
srun ./my_mpi_program
```

If a specific application documents another MPI launch method, follow the
requirements for that application.


## OpenMPI and Compilers

OpenMPI is built using a compiler toolchain.

A typical software stack looks like:

```text
Compiler
   ↓
OpenMPI
   ↓
MPI Application
```

Check the compiler used by the loaded OpenMPI:

```bash
mpicc --showme
```

You may see a GCC-based compiler or another supported compiler stack.

!!! warning "Do not mix incompatible MPI stacks"
    An application compiled with one OpenMPI/compiler combination should
    normally be run with the same compatible software stack.

Avoid situations such as:

```text
Compile:
GCC A + OpenMPI A

Run:
GCC B + OpenMPI B
```

unless the combinations are known to be compatible.


## Building with GCC

A typical GCC/OpenMPI workflow is:

```bash
module purge

module load gcc/VERSION
module load openmpi/VERSION
```

Check:

```bash
module list
mpicc --showme
```

Compile:

```bash
mpicc -O2 program.c -o program
```

[GCC :material-arrow-right:](gcc.md){ .md-button }


## Building with Intel oneAPI

If an OpenMPI build compatible with Intel oneAPI is available:

```bash
module purge

module load intel-oneapi-compilers/VERSION
module load openmpi/VERSION
```

Check the MPI wrapper:

```bash
mpicc --showme
```

Do not assume every OpenMPI module is compatible with every compiler.

[Intel oneAPI :material-arrow-right:](intel-oneapi.md){ .md-button }


## MPI + OpenMP Hybrid Jobs

Some applications use both:

```text
MPI
+
OpenMP
```

MPI distributes work between processes while OpenMP uses multiple threads
inside each process.

For example:

```bash
#SBATCH --nodes=2
#SBATCH --ntasks-per-node=4
#SBATCH --cpus-per-task=8
```

This creates:

```text
2 nodes
×
4 MPI ranks per node
×
8 threads per MPI rank
```

Set:

```bash
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
```

Then run:

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

#SBATCH --time=01:00:00
#SBATCH --mem=32G

module purge
module load openmpi/VERSION

export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK

echo "MPI ranks: $SLURM_NTASKS"
echo "Threads per rank: $OMP_NUM_THREADS"

srun ./hybrid_program
```

[OpenMP :material-arrow-right:](openmp.md){ .md-button }


## Interactive MPI Testing

For small tests, request an interactive compute allocation.

For example:

```bash
salloc \
  --nodes=2 \
  --ntasks=8 \
  --time=00:30:00
```

After the allocation starts:

```bash
module purge
module load openmpi/VERSION
```

Then:

```bash
srun ./hello_mpi
```

Exit the allocation when finished:

```bash
exit
```

!!! warning
    Do not run large MPI workloads directly on login nodes.


## Check Where MPI Ranks Are Running

A useful test program can print the hostname for each rank.

You can also run:

```bash
srun hostname
```

For a multi-node allocation, the output should show tasks distributed across
the allocated nodes.

Check the assigned nodes:

```bash
scontrol show job $SLURM_JOB_ID
```


## Common Problems

### `mpicc: command not found`

Check:

```bash
module avail openmpi
```

Load OpenMPI:

```bash
module load openmpi/VERSION
```

Verify:

```bash
which mpicc
mpicc --showme
```


### MPI Program Works on One Node but Fails Across Multiple Nodes

Check:

```bash
module list
```

and:

```bash
mpicc --showme
```

Make sure the program was compiled and executed with the same compatible MPI
stack.

Also verify the Slurm allocation:

```bash
scontrol show job $SLURM_JOB_ID
```


### Wrong Number of MPI Processes

Check:

```bash
echo "$SLURM_NTASKS"
```

and your job directives:

```bash
#SBATCH --nodes=...
#SBATCH --ntasks=...
#SBATCH --ntasks-per-node=...
```

Remember:

```text
--ntasks = total MPI ranks
```


### More Processes Than Allocated CPUs

Do not manually launch more MPI ranks than Slurm allocated.

For example, if the job requests:

```bash
#SBATCH --ntasks=8
```

run:

```bash
srun ./program
```

rather than manually requesting a larger process count.


### MPI Library Errors

Examples include:

```text
libmpi.so: cannot open shared object file
```

or:

```text
undefined symbol
```

Check:

```bash
module list
ldd ./program
```

Verify that the correct OpenMPI module is loaded.


### PMIx / PMI Errors

Errors mentioning:

```text
PMI
PMIx
```

may indicate a mismatch between:

- Slurm
- OpenMPI
- The MPI launch method
- The software stack used to build the application

Collect:

```bash
module list
mpirun --version
mpicc --showme
```

and the Slurm job ID before contacting HPC support.


## Best Practices

- Use `module avail openmpi` to find available versions.
- Compile MPI applications with `mpicc`, `mpicxx`, or `mpifort`.
- Use explicit OpenMPI versions for important workflows.
- Load the required MPI environment inside every Slurm job.
- Treat one Slurm task as one MPI rank for normal MPI workloads.
- Use `srun` as the normal Slurm launch method.
- Do not manually SSH between compute nodes.
- Do not launch more ranks than Slurm allocated.
- Do not mix incompatible compiler and MPI stacks.
- Use `OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK` for hybrid MPI/OpenMP jobs.
- Run MPI workloads on compute nodes, not login nodes.
- Record compiler and OpenMPI versions for reproducible research.


## Need Help?

Collect:

```bash
hostname
module list

which mpicc
mpicc --showme

which mpirun
mpirun --version
```

For a running or failed Slurm job, also provide:

```bash
scontrol show job JOBID
```

and include:

- Job ID
- Job script
- Output file
- Error file
- Number of nodes
- Number of MPI tasks
- Compiler used to build the application

For assistance:

[**hpcadmin@fiu.edu**](mailto:hpcadmin@fiu.edu)


## Related Guides

[Environment Modules](../modules/index.md){ .md-button }

[GCC](gcc.md){ .md-button }

[Intel oneAPI](intel-oneapi.md){ .md-button }

[OpenMP](openmp.md){ .md-button }

[Running Jobs](../../running-jobs/index.md){ .md-button }
