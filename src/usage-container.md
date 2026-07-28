# Building & running HADDOCK3 with Apptainer/Singularity (HPC)

Build, run, and extend an **Apptainer** container for HADDOCK3. [**HADDOCK3**](https://doi.org/10.1021/acs.jcim.5c00969) (High Ambiguity Driven protein–protein Docking) is a flexible, information-driven software suite for modeling biomolecular complexes using experimental and theoretical restraints. **Docker** and **Apptainer** are containerization platforms that package applications and all their dependencies into lightweight, portable images, ensuring reproducible execution across different environments.

A ready-to-use Docker image for HADDOCK3 is published on the GitHub container registry, simply pull:

```bash
# For the latest release
docker pull ghcr.io/haddocking/haddock3:latest

# Or pin to a specific release for reproducibility
docker pull ghcr.io/haddocking/haddock3:2025.8.0
```

See the [HADDOCK3 container package](https://github.com/haddocking/haddock3/pkgs/container/haddock3) for details and version tags. This image serves as the canonical, versioned distribution of HADDOCK3 and can be used across development, CI/CD, and cloud environments.

To use this as the foundation for HPC-friendly SIF images, build **Apptainer** or **Singularity** containers directly from the Docker image in a single step. For example, to create an image:

```bash
# Build Apptainer image
apptainer build haddock3_mpi.sif docker://ghcr.io/haddocking/haddock3:2025.11.0
```

**Note:** See [HPC / SLURM usage](#hpc--slurm-usage) below for detailed instructions, including the example `slurm_run_apptainer.sh` script, on how to run HADDOCK3 jobs in an HPC environment. On many HPC systems, building Apptainer images requires elevated privileges and may be restricted. In such cases, users can build the SIF image on a local system where Apptainer is installed and then transfer it to the HPC system for use.

**Version updates:** The HADDOCK3 image is regularly updated. Please check the tags at [ghcr.io/haddocking/haddock3](https://github.com/haddocking/haddock3/pkgs/container/haddock3) for the latest version.

---

## Build your own HADDOCK3 container

Containerization techniques enable highly reproducible, customizable, and scalable scientific workflows. If you want to understand how the container is built under the hood from scratch or customize it for your own workflows, follow these detailed steps:

1. **Clone**

   ```bash
   git clone https://github.com/haddocking/haddock3.git
   cd haddock3
   ```

2. **Build**

   A definition file is a blueprint that tells the containerization platform how to build the container. It specifies the base OS, software packages, environment variables, and custom setup steps, ensuring your container is reproducible and tailored to your workflow. Here is a ready-to-use `haddock3_mpi.def` definition file:

   ```
   Bootstrap: docker
   From: ubuntu:22.04

   %labels
       Maintainer: HADDOCK-GROUP
       Description: "MPI-ready HADDOCK3 container"

   %environment
       export PATH=/usr/local/bin:$PATH
       export LD_LIBRARY_PATH=/usr/lib/x86_64-linux-gnu/openmpi/lib:$LD_LIBRARY_PATH

   %arguments
       HADDOCK_VERSION=2025.9.1   # Example HADDOCK3 version (can be overridden during build)

   %post
       # Install core dependencies and MPI
       apt-get update && apt-get install -y \
           wget git build-essential cmake gfortran \
           libfftw3-dev libxml2-dev zlib1g-dev \
           openmpi-bin libopenmpi-dev \
           python3 python3-pip \
       && apt-get clean && rm -rf /var/lib/apt/lists/*

       # Install Python packages with a fixed HADDOCK3 version
       pip3 install --no-cache-dir --upgrade pip \
           mpi4py haddock3==${HADDOCK_VERSION}

   %runscript
       echo "Welcome to the MPI-ready HADDOCK3 container."
       exec "$@"
   ```

   Build the image from this definition file:

   ```bash
   apptainer build --build-arg HADDOCK_VERSION=2025.11.0 \
     haddock3_mpi.sif \
     haddock3_mpi.def
   ```

   **Tip:** Set the `HADDOCK_VERSION` build argument to specify which HADDOCK3 version to install. To create your own definition files to layer in additional packages, alternative MPI variants, Python libraries, and any domain-specific utilities, modify the `%post` section of `haddock3_mpi.def` before building.

3. **Verify & run**

   - **Shell**: interactive access

     ```bash
     apptainer shell haddock3_mpi.sif
     ```

   - **HADDOCK3**: verify installation

     ```bash
     apptainer shell haddock3_mpi.sif haddock3 --version
     ```

## HPC / SLURM usage

This section explains how to run HADDOCK3 jobs either interactively or via SLURM batch submission (MPI HPC ready).

### Run interactively with `srun`

To run HADDOCK3 in an interactive SLURM session:

```bash
srun --partition=compute \
     --nodes=1 \
     --ntasks-per-node=8 \
     --chdir=/path/to/haddock3/examples/docking-protein-protein \
     /path/to/haddock3_mpi.sif \
     haddock3 docking-protein-protein-mpi.cfg
```

**Note:** Adjust paths — change the `.cfg` file path and directory to match your file location.

**CPU allocation and container behavior:** No rebuild is required for the container — it will automatically use the number of CPUs assigned by SLURM. In the above example, `--nodes=1` allocates one compute node, and `--ntasks-per-node=8` allocates eight parallel tasks. The exact number of nodes and CPU cores you can request depends on your HPC configuration — please refer to your cluster documentation for resource allocation limits.

### Run as a batch job with `sbatch`

Submit your job to SLURM using the sample `slurm_run_apptainer.sh` script below:

```bash
#!/bin/bash

#SBATCH --job-name=HADDOCK3-docking
#SBATCH --output=HADDOCK3_%j.out
#SBATCH --nodes=3
#SBATCH --ntasks-per-node=120
#SBATCH --mem=256GB
#SBATCH --partition=compute

#Some values like `--nodes`,`--mem`  and `--ntasks-per-node` are variable and depend on your HPC configuration

echo "Starting HADDOCK3 Docking Job"
echo "SLURM_JOBID          = $SLURM_JOBID"
echo "SLURM_JOB_NODELIST   = $SLURM_JOB_NODELIST"
echo "SLURM_NNODES         = $SLURM_NNODES"
echo "SLURMTMPDIR          = $SLURMTMPDIR"
echo "Date                 = $(date)"
echo "Hostname             = $(hostname -s)"
echo "Working Directory    = $(pwd)"
echo "Submit Directory     = $SLURM_SUBMIT_DIR"

# Load necessary environment (optional depending on your HPC setup)
source /lustre/oneApi/setvars.sh
export OMP_NUM_THREADS=1

# Run HADDOCK3 via Apptainer
cd "${WORK_DIR}"

#For MPI runs, ensure that the `.cfg` file reflects the allocated resources, with `ncore` set to the total number of available cores (e.g. 3 x 120 = 360) and the execution mode set to `mpi`

apptainer run \
  /path/to/haddock3_image.sif \
  haddock3 your-docking-config.cfg

echo "HADDOCK3 Job Complete"
echo "Completed at: $(date)"
```

```bash
sbatch slurm_run_apptainer.sh
```

Customize the script and configuration paths as needed.

### MPI run mode

When using multiple CPUs or running across multiple nodes, **HADDOCK3 must be executed in MPI mode**. To ensure proper parallel execution, set the following parameters in your configuration (`.cfg`) file, adjusting the mode and number of cores:

```toml
mode = "mpi"
ncores = 8
```

This enables HADDOCK3 to distribute workloads efficiently across all allocated CPUs.

**Note:** When running multiple CPUs on a single node, MPI mode is not required; HADDOCK3 can be run in local mode.

---

## Resources & tutorials

- **Apptainer installation & usage**: Detailed installation instructions and usage examples can be found on the official docs: [apptainer.org/docs/admin/main/installation.html](https://apptainer.org/docs/admin/main/installation.html)
- **Official HADDOCK3 tutorials**: Visit the Bonvin lab's educational page for HADDOCK3 tutorials: [bonvinlab.org/education/HADDOCK3](https://www.bonvinlab.org/education/HADDOCK3/)
- **Source code & issues**: Explore the HADDOCK3 source code on GitHub: [github.com/haddocking/haddock3](https://github.com/haddocking/haddock3)

## Requirements

- **Host**: Linux system with Docker or Apptainer/Singularity installed for building container images (e.g. a local machine or Linux environment via WSL2).
- **Disk**: ≥ 2 GB free for building.
- **Python**: 3.10+ (inside container).
- **Permissions**: Root (sudo) privileges are required to build Apptainer images locally, but not to execute them.
- **SLURM-enabled HPC environment** for running the HPC/batch instructions above.
- A `.cfg` configuration file for HADDOCK3.

---

*Get ready for a seamless, reproducible workflow for HADDOCK3 runs!*
