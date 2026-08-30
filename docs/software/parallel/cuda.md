# CUDA

<div class="cuda-hero">

  <div class="cuda-hero-content">

    <span class="roary-page-eyebrow">
      GPU DEVELOPMENT
    </span>

    <h2>NVIDIA CUDA on Roary</h2>

    <p>
      CUDA provides the compiler, libraries, and development tools used to
      build GPU-accelerated applications for NVIDIA GPUs.
    </p>

    <p>
      On Roary, CUDA toolkits are provided through Environment Modules.
      GPU resources must still be requested through Slurm before running
      GPU workloads.
    </p>

  </div>

  <div class="cuda-hero-badge">
    <span>ROARY</span>
    <strong>CUDA</strong>
    <small>NVIDIA GPU</small>
  </div>

</div>


## Quick Start

Check available CUDA versions:

```bash
module avail cuda
```

Load a CUDA toolkit:

```bash
module load cuda/VERSION
```

Verify the CUDA compiler:

```bash
which nvcc
nvcc --version
```

Inside a GPU allocation, check the GPU:

```bash
nvidia-smi
```

!!! important
    Loading the CUDA module does **not** allocate a GPU.

    GPU resources must be requested through Slurm.


## CUDA Toolkit vs NVIDIA Driver

CUDA and the NVIDIA driver are related but different.

```text
NVIDIA Driver
      ↓
CUDA Toolkit / Runtime
      ↓
GPU Application
```

The NVIDIA driver is installed and managed on Roary GPU nodes.

The CUDA module provides development tools and libraries such as:

```text
nvcc
CUDA runtime libraries
CUDA headers
GPU development tools
```

Check the CUDA toolkit version:

```bash
nvcc --version
```

Check the NVIDIA driver and GPU:

```bash
nvidia-smi
```

!!! note
    The CUDA version displayed by `nvidia-smi` represents driver compatibility
    and may not be the same version as the CUDA toolkit loaded with
    Environment Modules.


## Finding CUDA

Check available versions:

```bash
module avail cuda
```

Load one:

```bash
module load cuda/VERSION
```

Check the environment:

```bash
module list
which nvcc
nvcc --version
```

For reproducible builds and Slurm jobs, use an explicit CUDA module version.


## Simple CUDA Program

Example CUDA source:

```cpp
#include <stdio.h>
#include <cuda_runtime.h>

__global__ void hello()
{
    printf("Hello from GPU thread %d\n", threadIdx.x);
}

int main()
{
    hello<<<1, 4>>>();

    cudaDeviceSynchronize();

    return 0;
}
```

Save it as:

```text
hello.cu
```

Load CUDA:

```bash
module load cuda/VERSION
```

Compile:

```bash
nvcc hello.cu -o hello_cuda
```

Run the executable **inside a GPU allocation**:

```bash
./hello_cuda
```


## Compiling CUDA Applications

CUDA programs normally use the NVIDIA CUDA compiler:

```bash
nvcc
```

Basic compilation:

```bash
nvcc program.cu -o program
```

Optimized build:

```bash
nvcc -O2 program.cu -o program
```

Check the compiler:

```bash
nvcc --version
```

For larger applications, follow the build instructions provided by the
software package.


## Requesting a GPU with Slurm

A CUDA application must run on a GPU compute node.

A typical Slurm request includes:

```bash
#SBATCH --gres=gpu:1
```

and the appropriate GPU partition and QOS for your allocation.

Example:

```bash
#!/bin/bash

#SBATCH --job-name=cuda-test
#SBATCH --output=cuda_%j.out
#SBATCH --error=cuda_%j.err

#SBATCH --partition=GPU_PARTITION
#SBATCH --qos=GPU_QOS

#SBATCH --gres=gpu:1
#SBATCH --cpus-per-task=4
#SBATCH --mem=16G
#SBATCH --time=01:00:00

module purge
module load cuda/VERSION

echo "======================================"
echo "Job ID       : $SLURM_JOB_ID"
echo "Node         : $(hostname)"
echo "CUDA version :"
nvcc --version
echo "======================================"

nvidia-smi

./hello_cuda
```

Replace:

```text
GPU_PARTITION
GPU_QOS
VERSION
```

with the values appropriate for your Roary GPU allocation.

Submit:

```bash
sbatch cuda_job.sh
```

!!! important
    GPU partitions and QOS access depend on the resources available to your
    account.

    See the Roary GPU guide for the currently supported GPU resources and
    job submission options.

[GPU Computing :material-arrow-right:](../../gpus/index.md){ .md-button .md-button--primary }


## Check the Assigned GPU

Inside a GPU job:

```bash
nvidia-smi
```

You can also check which GPU devices Slurm exposed to your job:

```bash
echo "$CUDA_VISIBLE_DEVICES"
```

Typical output might look like:

```text
0
```

or:

```text
0,1
```

Applications should use only the GPUs allocated to the Slurm job.


## Multiple GPUs

If an application supports multiple GPUs, request more than one:

```bash
#SBATCH --gres=gpu:2
```

Check:

```bash
echo "$CUDA_VISIBLE_DEVICES"
nvidia-smi
```

!!! warning
    Requesting multiple GPUs does not automatically make an application
    use them.

    The application itself must support multi-GPU execution.


## CUDA with Python and Conda

GPU-enabled Python software such as:

- PyTorch
- TensorFlow
- JAX
- CuPy

may use CUDA libraries provided by the Python environment itself.

For example, a Conda environment may include CUDA runtime libraries required
by a particular framework.

This does **not** replace the NVIDIA driver on the compute node.

Always verify framework compatibility with the Roary GPU driver environment.

[Conda / Miniconda :material-arrow-right:](../languages/conda.md){ .md-button }


## Do I Always Need to Load a CUDA Module?

Not necessarily.

If you are:

- Compiling CUDA source with `nvcc`
- Building software against a specific CUDA toolkit
- Running an application that explicitly requires a CUDA module

then load the appropriate CUDA module.

Some prebuilt Python packages may already include the CUDA runtime libraries
they require.

In those cases, loading another unrelated CUDA toolkit may not be necessary.

!!! warning
    Do not blindly combine different CUDA toolkits, Conda CUDA runtimes,
    and application libraries.

    Use the versions required by the application.


## CPU Compiler Compatibility

CUDA applications may also compile CPU-side C or C++ code.

Check the compiler environment:

```bash
module list
```

and:

```bash
nvcc --version
```

If the application requires a particular GCC version, load the compatible
compiler before building:

```bash
module purge
module load gcc/VERSION
module load cuda/VERSION
```

Then compile:

```bash
nvcc program.cu -o program
```

[GCC :material-arrow-right:](gcc.md){ .md-button }


## Common Problems

### `nvcc: command not found`

Check available CUDA modules:

```bash
module avail cuda
```

Load one:

```bash
module load cuda/VERSION
```

Verify:

```bash
which nvcc
nvcc --version
```


### `nvidia-smi` Shows No GPU

Make sure you are running inside a GPU allocation.

Check:

```bash
echo "$SLURM_JOB_ID"
echo "$CUDA_VISIBLE_DEVICES"
```

GPU workloads should not be run directly on login nodes.


### CUDA Application Reports No Device

Check:

```bash
nvidia-smi
```

and:

```bash
echo "$CUDA_VISIBLE_DEVICES"
```

Also verify that the job requested a GPU:

```bash
#SBATCH --gres=gpu:1
```


### CUDA Version Mismatch

Check the loaded toolkit:

```bash
nvcc --version
```

Check the driver:

```bash
nvidia-smi
```

Check loaded modules:

```bash
module list
```

Make sure the application, CUDA runtime, and NVIDIA driver are compatible.


### Application Works on CPU but Not GPU

Confirm the application is actually GPU-enabled.

Then check:

```bash
nvidia-smi
```

and:

```bash
echo "$CUDA_VISIBLE_DEVICES"
```

For Python frameworks, also verify GPU support from inside the environment.


## Best Practices

- Use `module avail cuda` to find installed CUDA toolkits.
- Use an explicit CUDA version for reproducible builds.
- Use `nvcc` to compile CUDA source files.
- Request GPUs through Slurm before running CUDA applications.
- Use `nvidia-smi` to inspect the assigned GPU.
- Check `CUDA_VISIBLE_DEVICES` inside GPU jobs.
- Do not run GPU workloads on login nodes.
- Do not request more GPUs than the application can use.
- Do not blindly mix unrelated CUDA runtime versions.
- Check compiler compatibility before building CUDA software.
- Record CUDA and compiler versions for important research workflows.


## Need Help?

Collect:

```bash
hostname
module list

which nvcc
nvcc --version

nvidia-smi

echo "$CUDA_VISIBLE_DEVICES"
```

For Slurm problems, also include:

- Job ID
- Job script
- Output file
- Error file
- GPU partition
- QOS
- Number of requested GPUs

For assistance:

[**hpcadmin@fiu.edu**](mailto:hpcadmin@fiu.edu)


## Related Guides

[GPU Computing](../../gpus/index.md){ .md-button .md-button--primary }

[Environment Modules](../modules/index.md){ .md-button }

[GCC](gcc.md){ .md-button }

[Conda / Miniconda](../languages/conda.md){ .md-button }

[Running Jobs](../../running-jobs/index.md){ .md-button }
