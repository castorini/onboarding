# Alliance Canada

### Get started

We have resource allocations from Alliance Canada for most of [their supercomputers](https://www.computecanada.ca/research-portal/accessing-resources/available-resources/). To request access, create an account on their [website](http://ccdb.computecanada.ca) and follow [these instructions](https://alliancecan.ca/en/services/advanced-research-computing/account-management/apply-account). Jimmy's CCRI is `ssi-230-01`.

+ The use of your institutional username as your CC username is encouraged.

+ Once you have submitted your application, you will receive your confirmation email. Don't forget to click on the confirmation link indicated in the email.

There are four main general-purpose clusters, Rorqual, Fir, Nibi, and Narval, all of which you can login to with your CC credentials: `ssh USER@CLUSTERNAME.alliancecan.ca`.
While the first three have 80GB H100 GPUs, Narval has 40GB A100s. Depending on your needs and resource availability in the clusters, you might have to switch between them.

[CC Documentation](<https://docs.alliancecan.ca/wiki/Technical_documentation>) provides all details. From `Resources` on the left hand side you can check available computing resources in each cluster. The rest of the document will be based on Rorqual, though the usage of the other clusters should be identical or near-identical.

### Nodes and file system

Each cluster has a few __login nodes__ and a large number of __computing nodes__. When you ssh into `rorqual.alliancecan.ca`, you will login to one of its login nodes (e.g., rorqual1 or rorqual5). Login nodes are only for running lightweight jobs (no more than 10 CPU-minutes and 4G RAM). Heavy jobs need to be __submitted__ through SLURM (see next section).

You can go to other nodes in the cluster by direct ssh. For example, if you are in rorqual1 and wish to go to rorqual5, simply run `ssh rorqual5`. When you have a job running on a computing node (e.g., rg21807), you can also `ssh rg21807` and check things there, such as process status and local log files.

There are three main parts of the file system: home, scratch, and projects. They are accessible from all nodes in the cluster. How they are and how I use them are as follows:

* home: small quota, daily backup. Used for dotfiles and virtual environments.
* scratch: large quota, fast, no backup (and even worse, inactive data will be purged). Used for storing __active__ project's code and data.
* projects: large quota, slow, daily backup. I move projects there when they are done.

Apart from these three, computing nodes have their own local storage, accessible by the environment variable `$SLURM_TMPDIR`, or `/localscratch/USER.SLURM_JOBID` when you ssh onto it. The local storage is much faster than the three general ones, and should be used if you have intensive IO demand.

### Submitting jobs through SLURM

SLURM is the job manager. You submit job requests from login nodes, SLURM allocates a computing node to the job, and the job runs in the computing job.

Always keep in mind that SLURM deals with a lot of requests, so (1) your jobs don't always get resources immediately and sometimes you have to wait for quite a while; (2) ask for as much resource as you need (or only slightly more) for higher queueing priority.

There are (at least) 2 ways to submit a job: interactive and non-interactive.

If your job's maximum time is longer than 24 hours, please submit that job in a non-interactive way because the time limit of interactive jobs is 24 hours.

* Interactive: you get a shell on the computing node and run jobs as usual. Useful when you are running experiments for the first time and need to debug here and there. Command:

  `srun --mem=32G --cpus-per-task=2 --time=24:0:0 --gres=gpu:h100:1 --pty zsh`

  Most arguments are self-explanatory.

  *  `time` is the upper bound for your job - if the job runs longer than what you specified, you will be kicked out of the computing node; however don't set it to much longer than needed, as that will lower your SLURM priority. If you are just debugging go with something less than 3 hours (for instance `1:42:00`).

  * `gres` specifies what and how many GPUs you need (check <https://docs.alliancecan.ca/wiki/Using_GPUs_with_Slurm>). Remove it if you don't need GPUs. If you want say 2 GPUs, change this to `gpu:h100:2` instead. Make sure you are actually utilizing all your GPUs using `nvidia-smi` while your experiments run.

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
  #SBATCH --mem=32G 
  #SBATCH --cpus-per-task=2 
  #SBATCH --time=24:0:0 
  #SBATCH --gres=gpu:h100:1
  
  export CUDA_VISIBLE_DEVICES=0  # 0,1 if you want to use say 2 gpus you ask for
  
  dataset=$1
  python my_job.py --dataset $dataset
  ```

  One more thing you can specify is `#SBATCH --output=OUTPUT_FILE`, and `OUTPUT_FILE` is where stdout and stderr go to. If it's not specified, they go to `slurm-SLURM_JOBID.out` in the directory of submission. Use `#SBATCH --error=ERR_FILE` if you want a separate file for stderr. Normally I simply set `SBATCH --output=logs/%j.out`, where `%j` will be automatically replaced with slurm jobid. However, if your job produces lots of output (for example, if you use tqdm, which is a mistake I once made), you'd better add `exec &> $SLURM_TMPDIR/$SLURM_JOBID.out` in `job.sh`, before running python, so that all outputs are redirected to the local storage, and then copy it over to permanent storage when the job is done.

  Then you can simply run `sbatch job.sh DATASET`. By putting your job inside a script like this, you can, in interactive mode, run the same thing by `./job.sh DATASET`, since `#SBATCH` options are neglected there.

  You can request for a whole node on SLURM, which can shorten wait times. Set these arguments:

  ```
  #!/bin/bash
  #SBATCH --mem=0
  #SBATCH --cpus-per-task=32
  #SBATCH --gres=gpu:h100:4
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

To create a virtual environment on Alliance Canada, use virtualenv.
This allows users to create virtual environments within which you can easily install Python packages.
For various reasons, Alliance Canada asks users not to install Anaconda on their clusters: (see <https://docs.alliancecan.ca/wiki/Anaconda/en>).

* Virtualenv: 
  ```
  module load python/3.12                       # load the version of your choice
  virtualenv --no-download ~/ENV                # To create a virtual environment, where ENV is the name of the environment
  source ~/ENV/bin/activate
  pip install --no-index --upgrade pip          # You should also upgrade pip
  pip install --no-index --upgrade setuptools   # You should also upgrade setuptools
  deactivate                                    # You can exit the current environment
  ```  
  
  Use `pip install PACKAGE --no-index` to install python packages. The `--no-index` option enables you to install only from the Alliance Canada wheels compiled by Alliance Canada staff to prevent issues with missing or conflicting dependencies (check <https://docs.alliancecan.ca/wiki/Python> to find more available packages). If you omit the `--no-index` option, pip will search both PyPI and local packages, and use the latest version.
  
  From time to time, packages provided by Alliance Canada cause dependency problems. To completely avoid Alliance Canada packages and only search for PyPI packages when using `pip`, reset the following two environment variables _before creating the virtualenv environment_:
  ```
  PYTHONPATH=""
  PIP_CONFIG_FILE=""
  ```


These are the basic ways to use the clusters. If you have any questions, reach out to [Technical Support](https://alliancecan.ca/en/services/advanced-research-computing/technical-support/getting-help) or someone from our group.

### Code example (Deepspeed)
Deepspeed allows models with massive amounts of parameters to be trained efficiently, across multiple nodes. The following example offers a quick tutorial to use deepspeed to train a model with multi GPUs distributed over multiple nodes: <https://docs.alliancecan.ca/wiki/Deepspeed>. 


### Useful links
* [Slurm job running](https://docs.alliancecan.ca/wiki/Running_jobs)
* [Alliance Canada for Machine Learning](https://docs.alliancecan.ca/wiki/AI_and_Machine_Learning)
* [Alliance Canada Wiki for PyTorch](https://docs.alliancecan.ca/wiki/PyTorch)
* [Using GPUs with Slurm](https://docs.alliancecan.ca/wiki/Using_GPUs_with_Slurm)
* [Available software](https://docs.alliancecan.ca/wiki/Available_software)
