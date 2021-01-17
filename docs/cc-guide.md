# Compute Canada

### Get started

We have resource allocations from Compute Canada for most of [their supercomputers](https://www.computecanada.ca/research-portal/accessing-resources/available-resources/). To request access, create an account on their [website](http://ccdb.computecanada.ca) and follow [these instructions](https://www.computecanada.ca/research-portal/account-management/apply-for-an-account/). Jimmy's CCRI is `ssi-230-01`.

+ The use of your institutional username as your CC username is encouraged.

+ Once you have submitted your application, you will receive your confirmation email. Don't forget to click on the confirmation link indicated in the email.

There are three main clusters, graham, cedar, and beluga, all of which you can login to with your CC credentials: `ssh USER@CLUSTERNAME.computecanada.ca`.

[CC Documentation](<https://docs.computecanada.ca/wiki/Compute_Canada_Documentation>) provides all details. From `Resources` on the left hand side you can check available computing resources in each cluster. The rest of the document will be based on cedar, which I have the most experience with, though other clusters are also very similar in terms of how to use them.

### Nodes and file system

Each cluster has a few __login nodes__ and a large number of __computing nodes__. When you ssh into `cedar.computecanada.ca`, you will login to one of its login nodes (e.g., cedar1 or cedar5). Login nodes are only for running lightweight jobs (no more than 10 CPU-minutes and 4G RAM). Heavy jobs need to be __submitted__ through SLURM (see next section).

You can go to other nodes in the cluster by direct ssh. For example, if you are in cedar1 and wish to go to cedar5, simpy run `ssh cedar5`. When you have a job running on a computing node (e.g., cdr1024), you can also `ssh cdr1024` and check things there, such as process status and local log files.

There are three main parts of the file system: home, scratch, and projects. They are accessible from all nodes in the cluster. How they are and how I use them are as follows:

* home: small quota, daily backup. Used for dotfiles and conda environments.
* scratch: large quota, fast, no backup (and even worse, inactive data will be purged). Used for storing __active__ project's code and data.
* projects: large quota, slow, daily backup. I move projects there when they are done.

Apart from these three, computing nodes have their own local storage, accessible by the environment variable `$SLURM_TMPDIR`, or `/localscratch/USER.SLURM_JOBID` when you ssh onto it. The local storage is much faster than the three general ones, and should be used if you have intensive IO demand.

### Submitting jobs through SLURM

SLURM is the job manager. You submit job requests from login nodes, SLURM allocate a computing node to the job, and the job runs in the computing job.

Always keep in mind that SLURM deals with a lot of requests, so (1) your jobs don't always get resources immediately and sometime you have to wait for quite a while; (2) ask for as much resource as you need (or only slightly more) for higher queueing priority.

There are (at least) 2 ways to submit a job: interactive and non-interactive.

* Interactive: you get a shell on the computing node and run jobs as usual. Useful when you are running experiments for the first time and need to debug here and there. Command:

  `srun --mem=64G --cpus-per-task=2 --time=24:0:0 --gres=gpu:v100l:2 --pty zsh`

  Most arguments are self-explanatory.

  *  `time` is the upper bound for your job - if the job runs longer than what you specified, you will be kicked out of the computing node; however don't set it to much longer than needed, as that will lower your SLURM priority.

  * `gres` specifies what and how many GPUs you need (check <https://docs.computecanada.ca/wiki/Using_GPUs_with_Slurm>). Remove it if you don't need GPUs.

  * The final `--pty zsh` is for using `zsh`. If you prefer bash (huh? why would anyone not like oh-my-zsh?) simply change this arg to `--pty bash`.

  * Normally you also need to specify which resource account (i.e., jimmy's account, not yours) to charge for your job. However, I doubt you'll need to change it for different jobs, so a more convenient way is to set the following environment variables:

    ```
    export SLURM_ACCOUNT=def-jimmylin
    export SBATCH_ACCOUNT=$SLURM_ACCOUNT
    export SALLOC_ACCOUNT=$SLURM_ACCOUNT
    ```

* Non-interactive: you don't get a shell. Useful when you are confident your code is bug-free and want to run experiments without monitoring it. The first thing to do is to put your job into a script, for example:

  ```
  #!/bin/bash
  
  dataset=$1
  python my_job.py --dataset $dataset
  ```

  Assume the script is `job.sh`. To submit it, run `sbatch [arguments] job.sh DATASET`, where `[arguments]` are the same with `srun` in interactive. `DATASET` here is an example how you can submit a bunch of slightly different jobs quickly without modifying the script itself.

  A more convenient way is to put these argument __inside__ `job.sh`:

  ```
  #!/bin/bash
  #SBATCH --mem=64G 
  #SBATCH --cpus-per-task=2 
  #SBATCH --time=24:0:0 
  #SBATCH --gres=gpu:v100l:2
  
  export CUDA_AVAILABLE_DEVICES=0,1  # so you can use both gpus you ask for
  
  dataset=$1
  python my_job.py --dataset $dataset
  ```

  One more thing you can specify is `#SBTATCH --output=OUTPUT_FILE`, and `OUTPUT_FILE` is where stdout and stderr go to. If it's not specified, they go to `slurm-SLURM_JOBID.out` in the directory of submission. Use `#SBATCH --error=ERR_FILE` if you want a separate file for stderr. Normally I simply set `SBATCH --output=logs/%j.out`, where `%j` will be automatically replaced with slurm jobid. However, if you job produces lots of output (for example, if you use tqdm, which is a mistake I once made), you'd better add `exec &> $SLURM_TMPDIR/$SLURM_JOBID.out`in `job.sh`, before running python, so that all outputs are redirected to the local storage, and then copy it over to permanent storage when the job is done.

  Then you can simply run `sbatch job.sh DATASET`. By putting your job inside a script like this, you can, in interactive mode, run the same thing by `./job.sh DATASET`, since `#SBATCH` options are neglected there.

  You can request for a whole node on SLURM, which can shorten wait times. Set these arguments:

  ```
  #!/bin/bash
  #SBATCH --mem=0
  #SBATCH --cpus-per-task=32
  #SBATCH --gres=gpu:v100l:4
  ```

SLURM provides a number of useful environment variables in both interactive and non-interactive modes. The most important two are:

* `SLURM_TMPDIR`: the fast local storage on the computing node
* `SLURM_JOBID`: the slurm job id



Other useful SLURM-related commands:

* `sinfo`: check cluster computation nodes
* `squeue`, especially `squeue -u USER`: check running jobs, how much resource was asked, whether they are running or not, how much time is left, which nodes are allocated to them, etc. You can ssh to nodes on which your jobs are running.
* `scancel JOB_ID`: cancel the job
* `module load PACKAGE`: make various software packages available, changes and sets environment variables. You may need `module load java` before running experiments.

### Create a virtual environment

There are two ways to create a virtual environment on Compute Canada: Virtualenv and Anaconda.
These tools allow users to create virtual environments within which you can easily install Python packages.
Compute Canada asks users not to install Anaconda on their clusters for various reasons, but there are very few cases where only conda installs work. Hence, use Anaconda if Virtualenv doesn't work.

* Virtualenv: 
  ```
  module load python/3.6                        # load the version of your choice
  virtualenv --no-download ~/ENV                # To create a virtual environment, where ENV is the name of the environment
  source ~/ENV/bin/activate
  pip install --no-index --upgrade pip          # You should also upgrade pip
  pip install --no-index --upgrade setuptools   # You should also upgrade setuptools
  deactivate                                    # You can exit the current environment
  ```  
  
  Use `pip install PACKAGE --no-index` to install python packages. The `--no-index` option enables you to install only from the Compute Canada wheels compiled by Compute Canada staff to prevent issues with missing or conflicting dependencies (check <https://docs.computecanada.ca/wiki/Python> to find more available packages). If you omit the `--no-index` option, pip will search both PyPI and local packages, and use the latest version.
  
* Anaconda: 
  One common workflow is to install Anaconda to your home directory for local Python package management, then save all data and models to the temporary `~/scratch` directory, which has a much higher disk quota.
  
  Please follow this guide to install Anaconda on CC: https://www.digitalocean.com/community/tutorials/how-to-install-anaconda-on-ubuntu-18-04-quickstart

* Miscellaneous:

  Some packages can only be installed by Anaconda for non-root users. For example, one way to install the `faiss` package through conda is:

  ```shell
  # CPU version only
  conda install faiss-cpu -c pytorch
  
  # GPU version
  conda install faiss-gpu cudatoolkit=8.0 -c pytorch # For CUDA8
  conda install faiss-gpu cudatoolkit=9.0 -c pytorch # For CUDA9
  conda install faiss-gpu cudatoolkit=10.0 -c pytorch # For CUDA10
  ```

These are the basic ways to use CC. If you have any questions, feel free to bug me (Jayden@slack) or check CC documentation.
