# Request New Software

<div class="request-software-hero">

  <div class="request-software-hero-content">

    <span class="roary-page-eyebrow">
      SOFTWARE REQUEST
    </span>

    <h2>Request Software for Roary</h2>

    <p>
      If the software you need is not available on Roary and is difficult
      to install in your own account, you can request a shared installation
      from the HPC team.
    </p>

    <p>
      Shared software is typically installed as an Environment Module so it
      can be used consistently by multiple researchers.
    </p>

  </div>

  <div class="request-software-hero-badge">
    <span>ROARY</span>
    <strong>REQUEST</strong>
    <small>SOFTWARE</small>
  </div>

</div>


## Check Roary First

Before submitting a request, check whether the software is already installed:

```bash
module avail SOFTWARE_NAME
```

You can also search the available modules:

```bash
module avail
```

If the software is already available, load it with:

```bash
module load SOFTWARE/VERSION
```

[Environment Modules :material-arrow-right:](../modules/index.md){ .md-button }


## When Should I Request Software?

Submit a software request when the application:

- Is not already available on Roary
- Will be used by multiple researchers
- Requires administrator privileges
- Has complex system dependencies
- Requires MPI or CUDA integration
- Is difficult to install or maintain in your own account
- Would be better provided as a shared Environment Module

For simpler personal installations, see:

[Installing Your Own Software :material-arrow-right:](installing-software.md){ .md-button }


## Information to Include

Please provide:

- **Software name**
- **Required version**
- **Official website or download page**
- **Installation documentation**
- **Research group or project**
- **Whether MPI support is required**
- **Whether GPU/CUDA support is required**
- **Any required libraries or dependencies**

Example:

```text
Software: ExampleApp
Version: 4.2.0
Website: https://example.org
Users: Research Group ABC
MPI required: Yes
GPU required: No
Additional notes: Requires GCC and OpenMPI
```


## Submit the Request

Send the software request to:

[**hpcadmin@fiu.edu**](mailto:hpcadmin@fiu.edu)

Use a clear subject such as:

```text
Roary Software Request - SOFTWARE_NAME
```


## Example Request

```text
Subject: Roary Software Request - ExampleApp

Hello HPC Team,

I would like to request ExampleApp version 4.2.0 for use on the
Roary cluster.

Software:
ExampleApp

Version:
4.2.0

Website:
https://example.org

Research group:
ABC Lab

MPI required:
Yes

GPU required:
No

Please let me know if any additional information is required.

Thank you.
```


## What Happens Next?

The HPC team will review:

- Software requirements
- Dependencies
- Compiler requirements
- MPI compatibility
- GPU/CUDA requirements
- Licensing restrictions
- Whether the software should be installed as a shared module

If approved, the software will typically be made available through the Roary
module system.

You can then find it with:

```bash
module avail SOFTWARE_NAME
```

and load it with:

```bash
module load SOFTWARE/VERSION
```


## Related Guides

[Environment Modules](../modules/index.md){ .md-button }

[Installing Your Own Software](installing-software.md){ .md-button }
