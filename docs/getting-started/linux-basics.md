# Linux Basics

Roary runs Linux.

You do not need to become a Linux administrator to use the cluster, but you should understand several basic commands.

---

## Where Am I?

```bash
pwd
```

Example:

```text
/home/username
```

---

## List Files

```bash
ls
```

Detailed listing:

```bash
ls -lh
```

Include hidden files:

```bash
ls -la
```

---

## Change Directory

Enter a directory:

```bash
cd directory_name
```

Return to your home directory:

```bash
cd ~
```

Move up one directory:

```bash
cd ..
```

---

## Create a Directory

```bash
mkdir analysis
```

Create nested directories:

```bash
mkdir -p project/results
```

---

## Copy Files

```bash
cp source destination
```

Copy a directory:

```bash
cp -r source_directory destination_directory
```

---

## Move or Rename

```bash
mv old_name new_name
```

---

## Remove Files

```bash
rm file.txt
```

Remove a directory and its contents:

```bash
rm -r directory_name
```

!!! warning "Be careful with rm"

    Linux normally does not provide an automatic recycle bin for files
    removed from the command line.

    Check your path carefully before deleting files.

---

## View a File

```bash
cat file.txt
```

For larger files:

```bash
less file.txt
```

Exit `less` by pressing:

```text
q
```

---

## Edit Files

Common terminal editors include:

```bash
vi filename
```

or:

```bash
vim filename
```

Open OnDemand may also provide browser-based text editing.

---

## Check Disk Usage

Current directory:

```bash
du -sh .
```

Another directory:

```bash
du -sh directory_name
```

---

## Find Files

```bash
find . -name "filename"
```

Example:

```bash
find . -name "*.slurm"
```

---

## Useful Environment Commands

Current username:

```bash
whoami
```

Current host:

```bash
hostname
```

Current date:

```bash
date
```

---

## Next Step

[Run Your First Slurm Job :material-arrow-right:](first-job.md){ .md-button .md-button--primary }
