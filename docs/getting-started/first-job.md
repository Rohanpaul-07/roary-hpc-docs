# Your First Slurm Job

Now that you can connect to Roary and understand the difference between login and compute nodes, you are ready to run your first job.

---

## Step 1 — Create a Tutorial Directory

```bash
mkdir -p ~/roary-tutorial
cd ~/roary-tutorial
```

Confirm your location:

```bash
pwd
```

---

## Step 2 — Create a Slurm Script

Create a new file:

```bash
vi hello.slurm
```

Add:

```bash
#!/bin/bash

#SBATCH --job-name=hello-roary
#SBATCH --output=hello_%j.out
#SBATCH --error=hello_%j.err
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1
#SBATCH --mem=1G
#SBATCH --time=00:05:00

echo "Hello from Roary!"
echo "Job ID: $SLURM_JOB_ID"
echo "Running on: $(hostname)"
echo "Started: $(date)"

sleep 10

echo "Finished: $(date)"
```

Save the file.

---

## Step 3 — Understand the Resource Request

| Directive | Meaning |
|---|---|
| `--job-name` | Name shown by Slurm |
| `--output` | Standard output file |
| `--error` | Error output file |
| `--ntasks=1` | One task |
| `--cpus-per-task=1` | One CPU |
| `--mem=1G` | 1 GB of memory |
| `--time=00:05:00` | Maximum runtime of five minutes |

`%j` is automatically replaced with the Slurm job ID.

---

## Step 4 — Submit the Job

```bash
sbatch hello.slurm
```

Expected output:

```text
Submitted batch job 123456
```

`123456` represents the job ID.

Every Slurm job receives a unique job ID.

---

## Step 5 — Check the Queue

```bash
squeue -u $USER
```

Possible output:

```text
JOBID    PARTITION    NAME          USER      ST   TIME
123456   ...          hello-roary   username  R    0:03
```

Common job states include:

| State | Meaning |
|---|---|
| `PD` | Pending |
| `R` | Running |
| `CG` | Completing |
| `CD` | Completed |

---

## Step 6 — Check Job History

After the job finishes:

```bash
sacct -j 123456
```

Replace `123456` with your actual job ID.

---

## Step 7 — View the Output

List your files:

```bash
ls -lh
```

You should see something similar to:

```text
hello.slurm
hello_123456.out
hello_123456.err
```

View the output:

```bash
cat hello_123456.out
```

Expected output:

```text
Hello from Roary!
Job ID: 123456
Running on: COMPUTE_NODE
Started: ...
Finished: ...
```

Notice that the hostname shown is a **compute node**, not the login node.

That confirms that Slurm executed the workload on allocated compute resources.

---

## Step 8 — Check Resource Usage

Use the Roary JobUsage utility:

```bash
/home/share/bin/jobusage 123456
```

Replace `123456` with your job ID.

JobUsage can help show:

- CPU utilization
- Memory utilization
- GPU utilization when allocated
- GPU memory usage when available

---

## Cancel a Job

To cancel a running or pending job:

```bash
scancel JOBID
```

Example:

```bash
scancel 123456
```

---

## Congratulations

You have completed the basic HPC workflow:

```text
Connect
   ↓
Prepare
   ↓
Submit
   ↓
Schedule
   ↓
Run
   ↓
Monitor
   ↓
Results
```

You are now ready to adapt Slurm scripts for real research workloads.

---

## Next Step

[Where to Go Next :material-arrow-right:](next-steps.md){ .md-button .md-button--primary }
