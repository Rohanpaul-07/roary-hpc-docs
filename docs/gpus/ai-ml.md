# AI & Machine Learning

<div class="gpu-hero">

  <div class="gpu-hero-content">

    <span class="roary-page-eyebrow">
      AI & MACHINE LEARNING
    </span>

    <h2>AI Workloads on Roary GPUs</h2>

    <p>
      Roary GPU resources can be used for machine learning and deep-learning
      frameworks such as PyTorch, TensorFlow, JAX, and other CUDA-enabled
      research applications.
    </p>

    <p>
      AI software environments should normally be managed using Conda or
      Apptainer rather than installed into the system Python environment.
    </p>

  </div>

  <div class="gpu-hero-badge">
    <span>ROARY</span>
    <strong>AI</strong>
    <small>GPU</small>
  </div>

</div>


## Recommended AI Workflow

```text
Create software environment
        ↓
Request GPU through Slurm
        ↓
Activate environment
        ↓
Verify GPU access
        ↓
Run training or inference
        ↓
Monitor resource usage
```


## Option 1 — Conda

Load Miniconda:

```bash
module avail miniconda
module load miniconda/VERSION
```

Prepare Conda:

```bash
source "$(conda info --base)/etc/profile.d/conda.sh"
```

Activate your AI environment:

```bash
conda activate "$HOME/conda-envs/my-ai-env"
```

Use the installation instructions provided by your AI framework.

[Conda / Miniconda :material-arrow-right:](../software/languages/conda.md){ .md-button }


## Option 2 — Apptainer

AI applications distributed as containers can be run with Apptainer.

For NVIDIA GPU access:

```bash
apptainer exec \
  --nv \
  ai-container.sif \
  COMMAND
```

Example:

```bash
apptainer exec \
  --nv \
  ai-container.sif \
  python train.py
```

[Apptainer :material-arrow-right:](../software/containers/apptainer.md){ .md-button }


## AI GPU Slurm Job

```bash
#!/bin/bash

#SBATCH --job-name=ai-training
#SBATCH --output=ai_%j.out
#SBATCH --error=ai_%j.err

#SBATCH --partition=GPU_PARTITION
#SBATCH --qos=GPU_QOS

#SBATCH --gres=gpu:1
#SBATCH --cpus-per-task=8
#SBATCH --mem=32G
#SBATCH --time=04:00:00

module purge
module load miniconda/VERSION

source "$(conda info --base)/etc/profile.d/conda.sh"

conda activate "$HOME/conda-envs/my-ai-env"

echo "======================================"
echo "Job ID : $SLURM_JOB_ID"
echo "Node   : $(hostname)"
echo "======================================"

nvidia-smi

python train.py
```

Submit:

```bash
sbatch ai_job.sh
```


## Verify PyTorch GPU Access

Inside the GPU job:

```bash
python -c "import torch; print(torch.cuda.is_available())"
```

If GPU support is working:

```text
True
```

Check the detected GPU:

```bash
python -c "import torch; print(torch.cuda.get_device_name(0))"
```


## Verify TensorFlow GPU Access

```bash
python -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"
```

A working environment should show at least one GPU device.


## Verify JAX GPU Access

```bash
python -c "import jax; print(jax.devices())"
```

The output should include a GPU device when JAX GPU support is configured
correctly.


## CUDA and AI Frameworks

Modern AI frameworks may provide some CUDA runtime libraries inside their
Conda or Python environments.

You do not always need to load a separate CUDA module.

Load CUDA when:

- Compiling CUDA software
- Building CUDA extensions
- The framework specifically requires a toolkit version

Do not load unrelated CUDA versions without checking the framework
requirements.

[CUDA :material-arrow-right:](../software/parallel/cuda.md){ .md-button }


## CPU and Memory for AI Jobs

AI workloads usually require CPU and system memory in addition to GPUs.

Example:

```bash
#SBATCH --gres=gpu:1
#SBATCH --cpus-per-task=8
#SBATCH --mem=32G
```

CPUs may be used for:

- Data loading
- Data preprocessing
- Tokenization
- Image augmentation
- Feeding batches to the GPU


## Multi-GPU Training

If your framework supports distributed training:

```bash
#SBATCH --gres=gpu:2
```

may be used.

!!! important
    Requesting multiple GPUs does not automatically make PyTorch,
    TensorFlow, JAX, or another framework use them.

    The application must be configured for multi-GPU execution.


## Monitor AI Jobs

### GPU Usage

While the job is running:

```bash
nvidia-smi
```

For continuous monitoring:

```bash
watch -n 2 nvidia-smi
```

Check:

- GPU utilization
- GPU memory usage
- Running GPU processes


### Complete Job Usage

Use Roary's `jobusage` tool:

```bash
/home/share/bin/jobusage JOBID
```

For example:

```bash
/home/share/bin/jobusage 123456
```

For live monitoring:

```bash
/home/share/bin/jobusage -w JOBID
```

This helps determine whether the CPUs, memory, and GPUs requested by your AI
job are actually being used.


### Open OnDemand

You can also review resource usage through the **HPC Usage** tool in
Roary Open OnDemand:

```text
https://hpclogin.fiu.edu
```

Open **HPC Usage** from the dashboard for a graphical view.


## Recommended AI Monitoring

```text
GPU
 ↓
nvidia-smi
 ↓
GPU utilization
GPU memory


Complete Slurm job
 ↓
/home/share/bin/jobusage JOBID
 ↓
CPU / memory / GPU usage


Graphical view
 ↓
Open OnDemand
 ↓
HPC Usage
```

!!! tip
    AI workloads often request large amounts of CPU, memory, and GPU
    resources.

    Check actual usage before increasing resource requests.


## Common Problems

### Framework does not detect the GPU

First check:

```bash
nvidia-smi
```

Then:

```bash
echo "$CUDA_VISIBLE_DEVICES"
```

If both are correct, check whether your framework was installed with GPU
support.


### PyTorch returns `False`

If:

```bash
python -c "import torch; print(torch.cuda.is_available())"
```

returns:

```text
False
```

the installed PyTorch environment may not have working GPU support.


### CUDA out of memory

Possible solutions include:

- Reduce batch size
- Reduce model size
- Reduce input size
- Reduce simultaneously loaded samples

Monitor memory with:

```bash
nvidia-smi
```


### GPU utilization is low

Check:

```bash
nvidia-smi
```

and:

```bash
/home/share/bin/jobusage JOBID
```

Possible causes include:

- Slow data loading
- Too few CPU workers
- Small batch size
- CPU preprocessing bottleneck
- Application not fully using GPU acceleration


## Best Practices

- Use Conda or Apptainer for AI environments.
- Request GPUs through Slurm.
- Verify GPU access before starting long training jobs.
- Request enough CPU resources for data loading.
- Use `nvidia-smi` to monitor GPU utilization and memory.
- Use `/home/share/bin/jobusage JOBID` to review complete job usage.
- Use `/home/share/bin/jobusage -w JOBID` for live monitoring.
- Use Open OnDemand **HPC Usage** for graphical monitoring.
- Adjust future CPU and memory requests based on actual usage.
- Avoid requesting multiple GPUs unless your framework can use them.
- Keep AI environments reproducible.
- Do not run training jobs on login nodes.


## Related Guides

[GPU Overview](index.md){ .md-button }

[Requesting & Using GPUs](using-gpus.md){ .md-button .md-button--primary }

[Conda / Miniconda](../software/languages/conda.md){ .md-button }

[Apptainer](../software/containers/apptainer.md){ .md-button }

[CUDA](../software/parallel/cuda.md){ .md-button }
