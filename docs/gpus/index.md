# GPU Computing

<div class="gpu-hero">

  <div class="gpu-hero-content">

    <span class="roary-page-eyebrow">
      GPU COMPUTING
    </span>

    <h2>Accelerated Computing on Roary</h2>

    <p>
      Roary provides NVIDIA GPU resources for workloads that can benefit
      from massively parallel computation.
    </p>

    <p>
      GPUs are commonly used for artificial intelligence, machine learning,
      scientific computing, molecular simulation, image processing, and
      other accelerated research workloads.
    </p>

  </div>

  <div class="gpu-hero-badge">
    <span>ROARY</span>
    <strong>GPU</strong>
    <small>ACCELERATED</small>
  </div>

</div>


## When Should I Use a GPU?

A GPU is useful when the application has been specifically designed to use
GPU acceleration.

Common examples include:

- Artificial intelligence
- Machine learning
- Deep learning
- CUDA applications
- Molecular dynamics
- Scientific simulations
- Image processing
- GPU-enabled data analysis

!!! important
    Requesting a GPU does not automatically make normal CPU software faster.

    The application itself must support GPU acceleration.


## CPU vs GPU

```text
CPU
 ↓
General computing
Serial and moderately parallel workloads

GPU
 ↓
Massively parallel computing
AI / ML / CUDA / accelerated applications
```


## Check GPU Resources

To see current GPU resources:

```bash
sinfo -o "%P %G %D %t"
```

The GPU partitions and QOS available to you depend on your Roary account
and allocation.


## GPU Software

Common GPU software includes:

```text
CUDA
PyTorch
TensorFlow
JAX
CuPy
GPU-enabled scientific applications
```

CUDA development is documented separately:

[CUDA :material-arrow-right:](../software/parallel/cuda.md){ .md-button }


## How GPU Jobs Work

```text
Request GPU through Slurm
          ↓
Slurm assigns a GPU node
          ↓
Application sees allocated GPU
          ↓
Run workload
          ↓
Monitor resource usage
```

GPU workloads should be launched through Slurm rather than by manually
connecting to GPU compute nodes.


## Requesting a GPU

A normal GPU request includes:

```bash
#SBATCH --gres=gpu:1
```

along with the appropriate GPU partition and QOS.

See:

[Requesting & Using GPUs :material-arrow-right:](using-gpus.md){ .md-button .md-button--primary }


## AI & Machine Learning

For PyTorch, TensorFlow, JAX, and other AI workloads:

[AI & Machine Learning :material-arrow-right:](ai-ml.md){ .md-button .md-button--primary }


## Monitoring GPU Jobs

While a GPU job is running, check the GPU with:

```bash
nvidia-smi
```

For continuous monitoring:

```bash
watch -n 2 nvidia-smi
```

Roary also provides the `jobusage` command for checking Slurm job resource
usage:

```bash
/home/share/bin/jobusage JOBID
```

For live monitoring:

```bash
/home/share/bin/jobusage -w JOBID
```

Replace `JOBID` with your Slurm job ID.

You can also review resource usage graphically using the **HPC Usage**
tool in Roary Open OnDemand:

```text
https://hpclogin.fiu.edu
```

Use these tools together:

```text
nvidia-smi
    ↓
GPU utilization and GPU memory

jobusage
    ↓
CPU, memory, GPU and job resource usage

Open OnDemand
    ↓
HPC Usage graphical view
```


## Best Practices

- Use GPUs only for software that supports GPU acceleration.
- Request only the number of GPUs your application can use.
- Run GPU workloads through Slurm.
- Use `nvidia-smi` to monitor GPU utilization and GPU memory.
- Use `/home/share/bin/jobusage JOBID` to review job usage.
- Use `/home/share/bin/jobusage -w JOBID` for live monitoring.
- Use the Open OnDemand **HPC Usage** tool for a graphical view.
- Check `CUDA_VISIBLE_DEVICES` inside GPU jobs.
- Use Conda or Apptainer for complex AI environments.
- Do not run GPU workloads on login nodes.


## Related Guides

[Requesting & Using GPUs](using-gpus.md){ .md-button }

[AI & Machine Learning](ai-ml.md){ .md-button }

[CUDA](../software/parallel/cuda.md){ .md-button }

[Conda / Miniconda](../software/languages/conda.md){ .md-button }

[Apptainer](../software/containers/apptainer.md){ .md-button }
