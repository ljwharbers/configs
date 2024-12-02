# nf-core/configs: VIB Data Core cluster

> **NB:** You will need access to VIB Data Core. Please visit https://docs.datacore.vib.be/ for more information

1. Install nextflow (with nf-core tools)

```bash
conda create --name nf-core python=3.12 nf-core nextflow
```

:::note
A nextflow module is also available that can be loaded `module load Nextflow`.
:::

2. Set up the environment variables in `~/.bashrc` or `~/.bash_profile`:

```bash
export SLURM_ACCOUNT="<your-credential-account>"

# Needed for running Nextflow jobs
export NXF_HOME="$SCRATCHDIR/.nextflow" #TODO: check scratch dir variable
export NXF_WORK="$SCRATCHDIR/work"

# Needed for running Apptainer containers
export APPTAINER_CACHEDIR="$SCRATCHDIR/.apptainer/cache"
export APPTAINER_TMPDIR="$SCRATCHDIR/.apptainer/tmp"

# Optional tower key
# export TOWER_ACCESS_TOKEN="<your_tower_access_token>"
# export NXF_VER="<version>"      # make sure it's larger then 24.04.0
```

3. Make the submission script.

> **NB:** you should go to the cluster you want to run the pipeline on. You can check what clusters have the most free space using following command `sinfo --cluster wice|genius`.

```bash
$ more job.pbs
#!/bin/bash -l
#SBATCH --account=...
#SBATCH --partition=gp_64C_128T_512GB
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1

# module load Nextflow # does not support plugins
conda activate nf-core

nextflow run <pipeline> -profile vib <Add your other parameters>
```

> **NB:** You have to specify your credential account, by setting `export SLURM_ACCOUNT="<your-credential-account>"` else the jobs will fail.