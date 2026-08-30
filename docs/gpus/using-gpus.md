# Requesting & Using GPUs

<div class="gpu-hero">

  <div class="gpu-hero-content">

    <span class="roary-page-eyebrow">
      GPU COMPUTING
    </span>

    <h2>Running GPU Jobs on Roary</h2>

    <p>
      GPU resources are requested through Slurm just like CPUs and memory.
      A job must request a GPU before running GPU-enabled software.
    </p>

  </div>

  <div class="gpu-hero-badge">
    <span>ROARY</span>
    <strong>GPU</strong>
    <small>SLURM</small>
  </div>

</div>


## Check GPU Resources

See current GPU resources:

```bash
sinfo -o "%P %G %D %t"
```

Your available GPU partitions and QOS depend on your Roary allocation.


## Request a GPU

The main Slurm GPU option is:

```bash
#SBATCH --gres=gpu:1
```

A GPU job also requires the appropriate partition and QOS:

```bash
#SBATCH --partition=GPU_PARTITION
#SBATCH --qos=GPU_QOS
#SBATCH --gres=gpu:1
```


## Basic GPU Job

```bash
#!/bin/bash

#SBATCH --job-name=gpu-test
#SBATCH --output=gpu_%j.out
#SBATCH --error=gpu_%j.err

#SBATCH --partition=GPU_PARTITION
#SBATCH --qos=GPU_QOS

#SBATCH --gres=gpu:1
#SBATCH --cpus-per-task=4
#SBATCH --mem=16G
#SBATCH --time=01:00:00

echo "======================================"
echo "Job ID : $SLURM_JOB_ID"
echo "Node   : $(hostname)"
echo "======================================"

nvidia-smi

echo "CUDA_VISIBLE_DEVICES=$CUDA_VISIBLE_DEVICES"

./gpu_application
```

Replace:

```text
GPU_PARTITION
GPU_QOS
```

with resources available to your account.

Submit:

```bash
sbatch gpu_job.sh
```


## Check the Assigned GPU

Inside the GPU job:

```bash
nvidia-smi
```

Check which GPU Slurm exposed:

```bash
echo "$CUDA_VISIBLE_DEVICES"
```


## Request Multiple GPUs

If your application supports multiple GPUs:

```bash
#SBATCH --gres=gpu:2
```

Then check:

```bash
nvidia-smi
```

and:

```bash
echo "$CUDA_VISIBLE_DEVICES"
```

!!! warning
    Requesting multiple GPUs does not automatically make an application
    use all of them.

    The application itself must support multi-GPU execution.


## Interactive GPU Session

For testing, request an interactive GPU allocation:

```bash
salloc \
  --partition=GPU_PARTITION \
  --qos=GPU_QOS \
  --gres=gpu:1 \
  --cpus-per-task=4 \
  --mem=16G \
  --time=01:00:00
```

After the allocation starts:

```bash
nvidia-smi
```

When finished:

```bash
exit
```


## Monitor GPU Usage

While the job is running:

```bash
nvidia-smi
```

For continuous GPU monitoring:

```bash
watch -n 2 nvidia-smi
```

This shows:

- GPU utilization
- GPU memory usage
- GPU processes
- GPU model
- Driver information


## Monitor the Entire Job

Roary provides the `jobusage` utility for checking the actual resources used
by a Slurm job.

Use:

```bash
/home/share/bin/jobusage JOBID
```

Example:

```bash
/home/share/bin/jobusage 123456
```

For a continuously updating view:

```bash
/home/share/bin/jobusage -w JOBID
```

Example:

```bash
/home/share/bin/jobusage -w 123456
```

Find your active job IDs with:

```bash
squeue -u "$USER"
```

The `jobusage` tool helps review resources such as:

- CPU usage
- Memory usage
- GPU utilization
- GPU memory usage
- Requested vs. used resources


## Open OnDemand HPC Usage

Resource usage is also available through the **HPC Usage** tool in
Roary Open OnDemand.

Open:

```text
https://hpclogin.fiu.edu
```

and select **HPC Usage** from the dashboard.

This provides a graphical way to review your resource usage.


## Recommended Monitoring Workflow

```text
GPU utilization
      ↓
nvidia-smi


Full job utilization
      ↓
/home/share/bin/jobusage JOBID


Live job monitoring
      ↓
/home/share/bin/jobusage -w JOBID


Graphical view
      ↓
Open OnDemand
      ↓
HPC Usage
```

!!! tip "Use actual usage to improve future jobs"
    If your job consistently uses much less CPU or memory than requested,
    reduce those requests in future jobs.

    If your workload reaches its limits, the usage information can help
    determine whether more resources are needed.


## GPU Memory

GPU memory is different from normal system RAM.

For example:

```bash
#SBATCH --mem=64G
```

requests system memory.

It does **not** increase GPU memory.

GPU memory is determined by the physical GPU assigned to the job.


## CPU Resources Still Matter

GPU applications normally also require CPU resources.

Example:

```bash
#SBATCH --gres=gpu:1
#SBATCH --cpus-per-task=8
#SBATCH --mem=32G
```

CPUs may be used for:

- Reading input data
- Preparing batches
- Data preprocessing
- Feeding data to the GPU
- Writing results


## Common Problems

### GPU is not visible

Check:

```bash
nvidia-smi
```

and:

```bash
echo "$CUDA_VISIBLE_DEVICES"
```

Make sure the job requested:

```bash
#SBATCH --gres=gpu:1
```


### CUDA out of memory

If you see:

```text
CUDA out of memory
```

possible solutions include:

- Reduce batch size
- Reduce model size
- Reduce input size
- Reduce simultaneously loaded data
- Use a GPU with more memory when available


### GPU utilization is low

Possible reasons include:

- Application is CPU-bound
- Slow data loading
- Small batch size
- Application does not fully use the GPU
- Excessive CPU/GPU synchronization

Check both:

```bash
nvidia-smi
```

and:

```bash
/home/share/bin/jobusage JOBID
```


## Best Practices

- Request GPUs through Slurm.
- Request only the number of GPUs your application can use.
- Use `nvidia-smi` to monitor GPU utilization.
- Use `/home/share/bin/jobusage JOBID` to review complete job usage.
- Use `/home/share/bin/jobusage -w JOBID` for live monitoring.
- Use Open OnDemand **HPC Usage** for graphical monitoring.
- Check `CUDA_VISIBLE_DEVICES`.
- Request enough CPU and memory to support the GPU workload.
- Adjust future Slurm requests based on actual usage.
- Do not run GPU workloads on login nodes.
- Use batch jobs for long-running GPU workloads.


## Related Guides

[GPU Overview](index.md){ .md-button }

[AI & Machine Learning](ai-ml.md){ .md-button }

[CUDA](../software/parallel/cuda.md){ .md-button }

[Running Jobs](../running-jobs/index.md){ .md-button }
