# Roary HPC Documentation

This repository contains the documentation for the **Roary HPC cluster** at Florida International University.

The website is built using **MkDocs** and **Material for MkDocs**.

## Quick Start

Clone the repository:

```bash
git clone https://github.com/Rohanpaul-07/roary-hpc-docs.git
cd roary-hpc-docs
```

Create a Python virtual environment:

```bash
python3 -m venv .venv
```

Activate it:

```bash
source .venv/bin/activate
```

Install the required packages:

```bash
pip install -r requirements.txt
```

## View the Website Locally

Start the local documentation server:

```bash
mkdocs serve
```

Then open:

```text
http://127.0.0.1:8000
```

The page will automatically refresh when you make changes to the documentation.

To stop the server, press:

```text
Ctrl+C
```

## Build the Website

Before pushing changes, check that the documentation builds correctly:

```bash
mkdocs build --strict
```

If there are no errors, the documentation is ready to commit.

## Where to Make Changes

Most documentation files are inside:

```text
docs/
```

The main navigation is controlled by:

```text
mkdocs.yml
```

Custom CSS is located at:

```text
docs/assets/stylesheets/extra.css
```

## Simple Workflow

Whenever you want to update the documentation:

```bash
source .venv/bin/activate
mkdocs serve
```

Make your changes inside `docs/`.

When finished, run:

```bash
mkdocs build --strict
```

Then commit and push:

```bash
git add .
git commit -m "Update documentation"
git push
```

## Files You Should Not Commit

Do not commit local or generated files such as:

```text
.venv/
site/
*.bak
*.bak.*
```

These should remain ignored through `.gitignore`.

## Help

For questions about the Roary HPC cluster, contact:

```text
hpcadmin@fiu.edu
```

FIU Instructional & Research Computing Center:

https://ircc.fiu.edu/
