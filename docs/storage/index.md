# Storage & Data

<div class="storage-hero">

  <div class="storage-hero-content">

    <span class="roary-page-eyebrow">
      ROARY RESEARCH STORAGE
    </span>

    <h2>
      Permanent working storage, high-performance scratch, and investor backup
    </h2>

    <p>
      Roary provides shared research storage for active projects,
      computational workloads, and research data.
    </p>

    <p>
      Choosing the correct storage location is important for performance,
      data retention, quota management, and protecting important results.
    </p>

  </div>

  <div class="storage-hero-badge">

    <span>ROARY</span>
    <strong>DATA</strong>
    <small>STORAGE</small>

  </div>

</div>


## Roary Storage Tiers

<div class="storage-tier-grid">

  <div class="storage-tier-card storage-tier-home">

    <div class="storage-tier-header">

      <span class="storage-code">
        HOME
      </span>

      <span class="storage-tier-state">
        PERMANENT
      </span>

    </div>

    <strong>
      /home
    </strong>

    <p>
      Permanent working storage for research data, scripts,
      software environments, project files, and results.
    </p>

    <div class="storage-purpose-tags">

      <span>Permanent</span>
      <span>Active Research</span>
      <span>Project Data</span>
      <span>Results</span>

    </div>

    <div class="storage-tier-note">
      Not automatically purged
    </div>

  </div>


  <div class="storage-tier-card storage-tier-scratch">

    <div class="storage-tier-header">

      <span class="storage-code">
        SCRATCH
      </span>

      <span class="storage-tier-state storage-tier-temp">
        TEMPORARY
      </span>

    </div>

    <strong>
      /scratch
    </strong>

    <p>
      High-performance temporary workspace for computational jobs,
      intermediate files, temporary datasets, and high-I/O workflows.
    </p>

    <div class="storage-purpose-tags">

      <span>Temporary</span>
      <span>Job I/O</span>
      <span>Intermediate Data</span>
      <span>High I/O</span>

    </div>

    <div class="storage-tier-note">
      Subject to the 30-day purge policy
    </div>

  </div>


  <div class="storage-tier-card storage-tier-backup">

    <div class="storage-tier-header">

      <span class="storage-code">
        BACKUP
      </span>

      <span class="storage-tier-state storage-tier-investor">
        INVESTOR
      </span>

    </div>

    <strong>
      /back-up
    </strong>

    <p>
      Backed-up storage is available to qualifying investor
      allocations that have purchased this storage service.
    </p>

    <div class="storage-purpose-tags">

      <span>Backed Up</span>
      <span>Investor Service</span>
      <span>Long-Term</span>

    </div>

    <div class="storage-tier-note">
      Investment required
    </div>

  </div>

</div>


<div class="storage-important-distinction">

  <div class="storage-warning-mark">
    !
  </div>

  <div>

    <span class="roary-page-eyebrow">
      IMPORTANT DISTINCTION
    </span>

    <h3>
      Permanent storage is not the same as backed-up storage
    </h3>

    <p>
      Files placed in <code>/home</code> are permanent in the sense that
      they are not automatically removed by the scratch purge policy.
    </p>

    <p>
      However, standard <code>/home</code> storage should not be considered
      a backup service. Backed-up storage is available only through qualifying
      investor storage allocations.
    </p>

  </div>

</div>


## `/home` — Permanent Working Storage

For most users, `/home` is the primary location for research work that
needs to remain available between jobs and login sessions.

Your home directory normally resembles:

```text
/home/USERNAME
```

Check your home directory:

```bash
echo $HOME
```

Typical uses include:

<div class="home-use-grid">

  <div>
    <span>CODE</span>
    <strong>Scripts & Source Code</strong>
  </div>

  <div>
    <span>DATA</span>
    <strong>Active Research Data</strong>
  </div>

  <div>
    <span>ENV</span>
    <strong>Software Environments</strong>
  </div>

  <div>
    <span>CONFIG</span>
    <strong>Configuration Files</strong>
  </div>

  <div>
    <span>RESULTS</span>
    <strong>Job Results</strong>
  </div>

  <div>
    <span>GROUP</span>
    <strong>Authorized Research Data</strong>
  </div>

</div>

Unlike `/scratch`, `/home` is **not subject to the automatic 30-day
scratch purge**.


## Need More Permanent Storage?

Storage allocations vary by research group.

If your research requires additional permanent storage beyond the
capacity assigned to your current allocation, additional storage is
provided through an **HPC investment**.

<div class="storage-invest-panel">

  <div class="storage-invest-mark">
    +
  </div>

  <div>

    <span class="roary-page-eyebrow">
      STORAGE INVESTMENT
    </span>

    <h3>
      Need additional storage capacity?
    </h3>

    <p>
      Additional storage capacity is investment-based.
      Contact the HPC Admins to discuss storage requirements,
      available options, and investment.
    </p>

    <a href="mailto:hpcadmin@fiu.edu">
      hpcadmin@fiu.edu
    </a>

  </div>

</div>


## Backup Storage

Backup storage is **not automatically included with a standard HPC account**.

It is available to qualifying **HPC investors** who have purchased
backed-up storage.

<div class="backup-investor-panel">

  <div>

    <span class="roary-page-eyebrow">
      INVESTOR STORAGE
    </span>

    <h3>
      Backed-up storage
    </h3>

    <p>
      The investor backup tier is intended for research data that requires
      a backed-up storage service and longer-term protection.
    </p>

  </div>

  <div class="backup-investor-status">

    <span>ACCESS</span>
    <strong>INVESTOR ONLY</strong>

  </div>

</div>

If your group needs backup storage, email:

[**hpcadmin@fiu.edu**](mailto:hpcadmin@fiu.edu)

to discuss storage investment options.


## `/scratch` — Temporary Computational Workspace

`/scratch` is intended for temporary computational work.

It is particularly useful for jobs that create substantial temporary
or intermediate data.

<div class="scratch-use-grid">

  <div class="scratch-use-card">

    <span>INPUT</span>

    <strong>
      Staged Job Data
    </strong>

    <p>
      Temporary copies of input data used during computation.
    </p>

  </div>


  <div class="scratch-use-card">

    <span>TMP</span>

    <strong>
      Intermediate Files
    </strong>

    <p>
      Data generated between stages of a computational workflow.
    </p>

  </div>


  <div class="scratch-use-card">

    <span>I/O</span>

    <strong>
      High-I/O Workloads
    </strong>

    <p>
      Temporary read/write intensive data generated by applications.
    </p>

  </div>


  <div class="scratch-use-card">

    <span>PIPE</span>

    <strong>
      Pipeline Workspace
    </strong>

    <p>
      Reproducible temporary data used by analysis pipelines.
    </p>

  </div>

</div>


<div class="scratch-purge-panel">

  <div class="scratch-purge-number">
    30
  </div>

  <div>

    <span class="roary-page-eyebrow">
      SCRATCH PURGE POLICY
    </span>

    <h3>
      Scratch data is automatically removed under the 30-day purge policy
    </h3>

    <p>
      Never keep the only copy of an important result in
      <code>/scratch</code>.
    </p>

    <p>
      Move results that need to be retained to permanent storage
      as soon as the computation is complete.
    </p>

  </div>

</div>


## Finding Your Scratch Location

Scratch storage may be associated with your research group or PI allocation.

Do **not** assume that creating an arbitrary directory such as:

```text
/scratch/USERNAME
```

is the correct location for your account.

Use the scratch path assigned to your research group.

If you are unsure which scratch directory belongs to your allocation,
email:

[**hpcadmin@fiu.edu**](mailto:hpcadmin@fiu.edu)


## Storage Lifecycle

<div class="storage-lifecycle">

  <div class="storage-life-card">

    <span>01</span>

    <strong>
      Permanent Working Data
    </strong>

    <code>/home</code>

    <p>
      Active research data that needs to remain available.
    </p>

  </div>


  <div class="storage-life-arrow">
    →
  </div>


  <div class="storage-life-card storage-life-work">

    <span>02</span>

    <strong>
      Computational Workspace
    </strong>

    <code>/scratch</code>

    <p>
      Temporary working and intermediate data.
    </p>

  </div>


  <div class="storage-life-arrow">
    →
  </div>


  <div class="storage-life-card">

    <span>03</span>

    <strong>
      Results
    </strong>

    <code>/home</code>

    <p>
      Move retained results back to permanent storage.
    </p>

  </div>


  <div class="storage-life-arrow">
    →
  </div>


  <div class="storage-life-card storage-life-backup">

    <span>04</span>

    <strong>
      Backup
    </strong>

    <code>Investor Service</code>

    <p>
      Backed-up storage when purchased as part of an investment.
    </p>

  </div>

</div>


## What Should Go Where?

<div class="storage-decision-grid">

  <div class="storage-decision-card">

    <span>
      /home
    </span>

    <strong>
      Permanent working data
    </strong>

    <div>

      <p>✓ Source code</p>
      <p>✓ Job scripts</p>
      <p>✓ Active research data</p>
      <p>✓ Software environments</p>
      <p>✓ Retained results</p>

    </div>

  </div>


  <div class="storage-decision-card storage-decision-scratch">

    <span>
      /scratch
    </span>

    <strong>
      Temporary computational data
    </strong>

    <div>

      <p>✓ Intermediate files</p>
      <p>✓ Temporary job data</p>
      <p>✓ High-I/O working data</p>
      <p>✓ Pipeline temporary files</p>
      <p>✓ Re-creatable data</p>

    </div>

  </div>


  <div class="storage-decision-card storage-decision-backup">

    <span>
      BACKUP
    </span>

    <strong>
      Investor-backed storage
    </strong>

    <div>

      <p>✓ Important research data</p>
      <p>✓ Data requiring backup</p>
      <p>✓ Long-term retained data</p>
      <p>✓ Investor allocations</p>
      <p>✓ Purchased capacity</p>

    </div>

  </div>

</div>


## Check Your Storage Usage

Roary provides tools for checking storage usage and limits.


### myquota

For a quick quota check:

```bash
myquota
```

This is the simplest command to start with when checking your available
storage.


## HPC Usage

Roary also provides the `hpcusage` utility.

It can provide information about HPC resource usage such as storage
usage associated with your user and research group.

However, `/home/share/bin` may not be in a user's shell `PATH`
by default.


### Add HPC utilities to your PATH

Run the following command once.

Replace `<USERNAME>` with your HPC username:

```bash
sed -i 's|PATH=$PATH:$HOME/bin|PATH=$PATH:$HOME/bin:/home/share/bin|' /home/<USERNAME>/.bash_profile
```

For example:

```bash
sed -i 's|PATH=$PATH:$HOME/bin|PATH=$PATH:$HOME/bin:/home/share/bin|' /home/abc123/.bash_profile
```

Then reload your shell profile:

```bash
source ~/.bash_profile
```

Now run:

```bash
hpcusage
```

You can confirm that the command is available with:

```bash
which hpcusage
```

It should resolve from:

```text
/home/share/bin/hpcusage
```


<div class="hpcusage-tip">

  <span class="roary-page-eyebrow">
    ROARY UTILITY
  </span>

  <h3>
    Add /home/share/bin once
  </h3>

  <p>
    After adding <code>/home/share/bin</code> to your shell PATH,
    Roary utilities such as <code>hpcusage</code> can be called
    directly from your terminal.
  </p>

</div>


## Quotas

Storage limits may be associated with an individual user,
a research group, or an investor allocation.

Because allocations are different, the documentation should not assume
one universal storage quota for every Roary user.

<div class="quota-type-grid">

  <div>

    <span>USER</span>

    <strong>
      User Usage
    </strong>

    <p>
      Storage associated with your individual HPC account.
    </p>

  </div>


  <div>

    <span>GROUP</span>

    <strong>
      Research Group
    </strong>

    <p>
      Storage associated with your research allocation.
    </p>

  </div>


  <div>

    <span>INVESTOR</span>

    <strong>
      Purchased Capacity
    </strong>

    <p>
      Additional storage associated with HPC investment.
    </p>

  </div>

</div>


## Quota vs Filesystem Capacity

The amount of free space on the overall filesystem is not necessarily
the amount of space available to your user.

Check the filesystem:

```bash
df -h /home /scratch
```

Check your allocation:

```bash
myquota
```

or:

```bash
hpcusage
```

<div class="quota-vs-space">

  <div>

    <span>FILESYSTEM</span>

    <strong>
      Total Storage System
    </strong>

    <code>df -h</code>

    <p>
      Shows overall filesystem capacity.
    </p>

  </div>


  <div>

    <span>ALLOCATION</span>

    <strong>
      Your Available Storage
    </strong>

    <code>myquota / hpcusage</code>

    <p>
      Shows the limits associated with your account or group.
    </p>

  </div>

</div>


## Check Directory Usage

Check the current directory:

```bash
du -sh .
```

Check a particular project:

```bash
du -sh ~/project
```

Show the sizes of top-level items:

```bash
du -sh ./* 2>/dev/null | sort -h
```

!!! note "Large scans can be expensive"

    `du` must inspect filesystem metadata.

    Avoid repeatedly running broad scans against extremely large project
    trees or directories containing millions of files.


## Moving Data to and from Roary

The best transfer method depends on the amount of data being moved.

<div class="transfer-method-grid">

  <div class="transfer-method-card">

    <span>SMALL</span>

    <strong>
      SCP
    </strong>

    <p>
      Convenient for individual files and relatively small transfers.
    </p>

    <code>
      scp
    </code>

  </div>


  <div class="transfer-method-card">

    <span>REPEATED</span>

    <strong>
      rsync
    </strong>

    <p>
      Useful for directory trees and transfers that may need to be resumed
      or repeated.
    </p>

    <code>
      rsync
    </code>

  </div>


  <div class="transfer-method-card transfer-globus-card">

    <span>LARGE</span>

    <strong>
      Globus
    </strong>

    <p>
      Recommended for very large research datasets and high-volume
      data transfers.
    </p>

    <code>
      Globus
    </code>

  </div>

</div>


## Small File Transfers with SCP

Upload a file from your local computer:

```bash
scp FILE USERNAME@hpclogin.fiu.edu:/home/USERNAME/
```

Example:

```bash
scp analysis.py abc123@hpclogin.fiu.edu:/home/abc123/
```

Download a file:

```bash
scp USERNAME@hpclogin.fiu.edu:/home/USERNAME/results.txt .
```


## Repeated Transfers with rsync

Upload a project directory:

```bash
rsync -avh --progress PROJECT/ \
USERNAME@hpclogin.fiu.edu:/home/USERNAME/PROJECT/
```

Download results:

```bash
rsync -avh --progress \
USERNAME@hpclogin.fiu.edu:/home/USERNAME/results/ \
./results/
```

For interrupted transfers:

```bash
rsync -avh --partial --progress SOURCE DESTINATION
```


## Large Data Transfers — Use Globus

For large research datasets, use **Globus** instead of relying on
browser uploads or long-running SCP transfers.

Globus is designed for reliable, high-volume research data movement and
is particularly appropriate for transfers involving hundreds of gigabytes
or terabytes of data.

<div class="globus-panel">

  <div class="globus-mark">
    G
  </div>

  <div>

    <span class="roary-page-eyebrow">
      LARGE DATA TRANSFER
    </span>

    <h3>
      Use Globus for large datasets
    </h3>

    <p>
      Globus provides managed, fault-tolerant transfers and is the
      recommended approach when moving large research datasets to
      or from Roary.
    </p>

    <p>
      Open Globus and locate the FIU HPC collection/endpoint
      available for Roary data transfer.
    </p>

    <a
      href="https://www.globus.org/"
      target="_blank"
      rel="noopener">
      Open Globus ↗
    </a>

  </div>

</div>

If you need assistance identifying the correct FIU HPC Globus
collection or configuring a large transfer, email:

[**hpcadmin@fiu.edu**](mailto:hpcadmin@fiu.edu)


## Open OnDemand File Transfers

For smaller files, Open OnDemand also provides browser-based file
management:

```text
https://hpclogin.fiu.edu
```

The Files interface allows you to:

<div class="ood-storage-actions">

  <span>Browse</span>
  <span>Upload</span>
  <span>Download</span>
  <span>Rename</span>
  <span>Create Directories</span>
  <span>Edit Text Files</span>

</div>

For large datasets, use **Globus** instead of the web browser.


## Research Group Storage

Research data may be owned by or shared through a research-group
allocation.

Check ownership and permissions:

```bash
ls -ld DIRECTORY
```

Example:

```text
drwxr-s--- username researchgroup project
```

The group associated with a directory determines which authorized
research-group members may access it.


## File Permissions

Check permissions:

```bash
ls -l FILE
```

or:

```bash
ls -ld DIRECTORY
```

Linux permissions are divided into:

<div class="permission-grid">

  <div>
    <span>r</span>
    <strong>Read</strong>
  </div>

  <div>
    <span>w</span>
    <strong>Write</strong>
  </div>

  <div>
    <span>x</span>
    <strong>Execute / Enter</strong>
  </div>

</div>


### User write permission

```bash
chmod u+w FILE
```


### Group read permission

```bash
chmod g+r FILE
```


### Group read/write permission

```bash
chmod g+rw FILE
```


### Change group when authorized

```bash
chgrp GROUP FILE
```


<div class="permission-warning">

  <div>
    !
  </div>

  <section>

    <span class="roary-page-eyebrow">
      PERMISSIONS
    </span>

    <h3>
      Do not use chmod 777 as a routine fix
    </h3>

    <p>
      Broad permissions may expose research data to users who do not
      need access.
    </p>

    <p>
      Use the minimum permissions required for the research workflow.
    </p>

  </section>

</div>


## Need Access to Another Research Group?

If you already have a Roary account but need access to another
research group's data or allocation, email the HPC Admins:

[**hpcadmin@fiu.edu**](mailto:hpcadmin@fiu.edu)

Include your HPC username and the research group you need to access.


## Organizing Your Project

A clean project structure makes jobs easier to reproduce and troubleshoot.

For example:

```text
project/
├── README.md
├── scripts/
├── input/
├── config/
├── logs/
├── results/
└── archive/
```

A data-intensive workflow might use:

```text
project/
├── raw_data/
├── reference/
├── scripts/
├── jobs/
├── logs/
├── intermediate/
└── results/
```

Keep original data, scripts, logs, temporary files, and final results
clearly separated.


## Slurm Output Files

Organize Slurm output rather than allowing large numbers of `.out`
and `.err` files to accumulate in the project root.

Example:

```bash
mkdir -p logs
```

Job script:

```bash
#SBATCH --output=logs/job_%j.out
#SBATCH --error=logs/job_%j.err
```

`%j` is replaced automatically with the Slurm job ID.


## Temporary Application Files

Some research applications generate large temporary files.

If the application supports a temporary directory setting, point those
temporary files to the scratch area assigned to your research group.

For applications that use `TMPDIR`, the pattern is:

```bash
export TMPDIR="YOUR_ASSIGNED_SCRATCH_PATH/job_${SLURM_JOB_ID}"
mkdir -p "$TMPDIR"
```

Replace:

```text
YOUR_ASSIGNED_SCRATCH_PATH
```

with the scratch location assigned to your research allocation.

Do not assume a scratch path if you do not know the correct location.


## Example Scratch Job Pattern

```bash
#!/bin/bash

#SBATCH --job-name=scratch-example
#SBATCH --output=logs/scratch_%j.out
#SBATCH --error=logs/scratch_%j.err
#SBATCH --cpus-per-task=4
#SBATCH --mem=8G
#SBATCH --time=01:00:00

SCRATCH_BASE="YOUR_ASSIGNED_SCRATCH_PATH"
WORKDIR="${SCRATCH_BASE}/${SLURM_JOB_ID}"

mkdir -p "$WORKDIR"

echo "Working directory: $WORKDIR"

# Copy required working data
cp "$SLURM_SUBMIT_DIR/input.dat" "$WORKDIR/"

cd "$WORKDIR"

# Run application here
# my_application input.dat > result.dat

# Return to submission directory
cd "$SLURM_SUBMIT_DIR"

mkdir -p results

# Copy retained results out of scratch
cp "$WORKDIR/result.dat" results/

# Clean temporary job workspace when appropriate
rm -rf "$WORKDIR"
```

!!! important

    Replace `YOUR_ASSIGNED_SCRATCH_PATH` with the actual scratch directory
    assigned to your research group.


## Managing Large Numbers of Files

A directory containing millions of small files can create substantial
filesystem metadata overhead.

This can result in:

<div class="small-file-grid">

  <span>Slow directory listings</span>
  <span>Slow searches</span>
  <span>Slow copies</span>
  <span>Long cleanup operations</span>
  <span>File-count limits</span>
  <span>Metadata pressure</span>

</div>

When appropriate, consolidate collections of small files.


## Archives

Create a tar archive:

```bash
tar -cf dataset.tar dataset/
```

Compress it:

```bash
tar -czf dataset.tar.gz dataset/
```

Inspect an archive:

```bash
tar -tf dataset.tar
```

Extract:

```bash
tar -xf dataset.tar
```


## Avoid Filesystem-Wide Searches

Do not search the entire shared filesystem when you only need your
own project.

Avoid broad commands such as:

```bash
find /home -type f
```

when a project-specific search will work.

Prefer:

```bash
find ~/project -name "*.out"
```


## Find Large Files

Example:

```bash
find . -type f -size +10G -ls
```

Show top-level usage:

```bash
du -sh ./* 2>/dev/null | sort -h
```


## Deleting Files

Delete one file:

```bash
rm FILE
```

Delete an empty directory:

```bash
rmdir DIRECTORY
```

Delete a directory tree:

```bash
rm -r DIRECTORY
```

<div class="delete-warning">

  <div class="delete-warning-mark">
    !
  </div>

  <div>

    <span class="roary-page-eyebrow">
      DELETION
    </span>

    <h3>
      Verify the path before deleting data
    </h3>

    <p>
      Files removed from HPC storage using command-line tools should be
      treated as permanently deleted.
    </p>

    <p>
      Be especially careful with recursive commands such as
      <code>rm -r</code> and <code>rm -rf</code>.
    </p>

  </div>

</div>


## Before a Large Workflow

<div class="storage-job-checklist">

  <div>
    <span>01</span>
    <strong>Check quota</strong>
    <code>myquota</code>
  </div>

  <div>
    <span>02</span>
    <strong>Check HPC usage</strong>
    <code>hpcusage</code>
  </div>

  <div>
    <span>03</span>
    <strong>Estimate data size</strong>
    <code>du -sh</code>
  </div>

  <div>
    <span>04</span>
    <strong>Select storage</strong>
    <code>/home or scratch</code>
  </div>

  <div>
    <span>05</span>
    <strong>Select transfer method</strong>
    <code>scp / rsync / Globus</code>
  </div>

  <div>
    <span>06</span>
    <strong>Plan retained results</strong>
    <code>copy out of scratch</code>
  </div>

</div>


## After a Large Workflow

<div class="storage-job-checklist">

  <div>
    <span>01</span>
    <strong>Verify results</strong>
    <code>ls -lh</code>
  </div>

  <div>
    <span>02</span>
    <strong>Check usage</strong>
    <code>du -sh</code>
  </div>

  <div>
    <span>03</span>
    <strong>Move retained results</strong>
    <code>scratch → /home</code>
  </div>

  <div>
    <span>04</span>
    <strong>Transfer large results</strong>
    <code>Globus</code>
  </div>

  <div>
    <span>05</span>
    <strong>Clean temporary files</strong>
    <code>scratch cleanup</code>
  </div>

  <div>
    <span>06</span>
    <strong>Review storage need</strong>
    <code>myquota / hpcusage</code>
  </div>

</div>


## Common Storage Problems

<div class="storage-problem-grid">

  <div>

    <span>QUOTA</span>

    <strong>
      Quota exceeded
    </strong>

    <p>
      Your user or research-group storage allocation may have reached
      its limit.
    </p>

  </div>


  <div>

    <span>CAPACITY</span>

    <strong>
      Need more storage
    </strong>

    <p>
      Additional permanent capacity requires an HPC storage investment.
    </p>

  </div>


  <div>

    <span>PERMISSION</span>

    <strong>
      Permission denied
    </strong>

    <p>
      Your user may not be associated with the required research group.
    </p>

  </div>


  <div>

    <span>SCRATCH</span>

    <strong>
      Scratch files missing
    </strong>

    <p>
      Temporary scratch data may have been removed under the purge policy.
    </p>

  </div>


  <div>

    <span>FILES</span>

    <strong>
      Too many small files
    </strong>

    <p>
      Very large numbers of files can create filesystem overhead.
    </p>

  </div>


  <div>

    <span>TRANSFER</span>

    <strong>
      Large transfer failing
    </strong>

    <p>
      Use Globus for large datasets instead of relying on a browser
      or long SCP session.
    </p>

  </div>

</div>


## Storage Rules to Remember

<div class="storage-final-rules">

  <div>
    <span>01</span>
    <strong>/home is permanent working storage.</strong>
  </div>

  <div>
    <span>02</span>
    <strong>Permanent does not mean backed up.</strong>
  </div>

  <div>
    <span>03</span>
    <strong>/scratch is temporary and subject to the 30-day purge.</strong>
  </div>

  <div>
    <span>04</span>
    <strong>Move retained results from scratch back to /home.</strong>
  </div>

  <div>
    <span>05</span>
    <strong>Backed-up storage is an investor service.</strong>
  </div>

  <div>
    <span>06</span>
    <strong>Additional permanent storage requires investment.</strong>
  </div>

  <div>
    <span>07</span>
    <strong>Use myquota and hpcusage to monitor storage.</strong>
  </div>

  <div>
    <span>08</span>
    <strong>Use Globus for large research datasets.</strong>
  </div>

  <div>
    <span>09</span>
    <strong>Avoid unnecessary millions of small files.</strong>
  </div>

  <div>
    <span>10</span>
    <strong>Contact HPC Admins for storage investment or access problems.</strong>
  </div>

</div>


## Need Help or More Storage?

For storage allocation questions, investment, backup storage,
research-group access, or Globus assistance, email:

[**hpcadmin@fiu.edu**](mailto:hpcadmin@fiu.edu)

When reporting a storage problem, include:

- Your HPC username
- Your research group or PI
- The filesystem involved
- The exact directory path
- The command you ran
- The complete error message
- The Slurm job ID if the problem occurred during a job

Never include your password.


## Next Steps

<div class="storage-next-grid">

  <a href="../running-jobs/">

    <span>SLURM</span>

    <strong>Running Jobs</strong>

    <small>Compute workflows →</small>

  </a>


  <a href="../software/">

    <span>SOFTWARE</span>

    <strong>Applications</strong>

    <small>Software environments →</small>

  </a>


  <a href="../open-ondemand/">

    <span>OOD</span>

    <strong>File Browser</strong>

    <small>Browser-based access →</small>

  </a>


  <a href="../troubleshooting/">

    <span>HELP</span>

    <strong>Troubleshooting</strong>

    <small>Storage problems →</small>

  </a>

</div>
