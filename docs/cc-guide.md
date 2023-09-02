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

If your job's maximum time is longer than 24 hours, please submit that job in a non-interactive way because the time limit of interactive jobs is 24 hours.

* Interactive: you get a shell on the computing node and run jobs as usual. Useful when you are running experiments for the first time and need to debug here and there. Command:

  `srun --mem=32G --cpus-per-task=2 --time=24:0:0 --gres=gpu:v100l:1 --pty zsh`

  Most arguments are self-explanatory.

  *  `time` is the upper bound for your job - if the job runs longer than what you specified, you will be kicked out of the computing node; however don't set it to much longer than needed, as that will lower your SLURM priority. If you are just debugging go with something less than 3 hours (for instance `1:42:00`).

  * `gres` specifies what and how many GPUs you need (check <https://docs.computecanada.ca/wiki/Using_GPUs_with_Slurm>). Remove it if you don't need GPUs. If you want say 2 GPUs, change this to `gpu:v100l:2` instead. Make sure you are actually utilizing all your GPUs using `nvidia-smi` while your experiments run.

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
  #SBATCH --gres=gpu:v100l:1
  
  export CUDA_AVAILABLE_DEVICES=0  # 0,1 if you want to use say 2 gpus you ask for
  
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
Compute Canada asks users not to install Anaconda on their clusters for various reasons, but there are cases where only conda installs work. Hence, use Anaconda if Virtualenv doesn't work.

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
  
  From time to time, packages provided by Compute Canada cause dependency problems. To completely avoid Compute Canada packages and only search for PyPI packages when using `pip`, reset the following two environment variables _before creating the virtualenv/conda environment_:
  ```
  PYTHONPATH=""
  PIP_CONFIG_FILE=""
  ```
  
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

### Code example (PyTorch)
The DistributedDataParallel class (or DDP) is [recommended by PyTorch maintainers](https://pytorch.org/tutorials/intermediate/ddp_tutorial.html#comparison-between-dataparallel-and-distributeddataparallel) to use multiple GPUs, whether they are all on a single node, or distributed across multiple nodes.

The following is a minimal working code to run a noop DDP job across 4 nodes.

#### sbatch.sh
```bash
#!/bin/bash
#SBATCH --nodes=4           # total nodes
#SBATCH --gres=gpu:1        # how many GPUs per node
#SBATCH --cpus-per-task=2   # Cores proportional to GPUs: 6 on Cedar, 16 on Graham.
#SBATCH --mem=32gb          # Memory proportional to GPUs: 32000 Cedar, 64000 Graham.
#SBATCH --time=0-00:10
#SBATCH --output=%N-%j.out

export NCCL_BLOCKING_WAIT=1  # Set this variable to use the NCCL backend

export SLURM_ACCOUNT=def-jimmylin
export SBATCH_ACCOUNT=$SLURM_ACCOUNT
export SALLOC_ACCOUNT=$SLURM_ACCOUNT

set -x
srun python pytorch-test.py tcp://$(hostname):8921
```

#### pytorch-test.py

```python
import os
import fire
import torch
import time
import datetime
import torch.distributed as dist
from torch.nn.parallel import DistributedDataParallel as DDP

def main(init_method):
    ngpus_per_node = torch.cuda.device_count()
    n_tasks = int(os.environ.get("SLURM_NTASKS"))
    local_id = int(os.environ.get("SLURM_LOCALID"))
    proc_id = int(os.environ.get("SLURM_PROCID"))
    job_id = int(os.environ.get("SLURM_JOBID"))
    n_nodes = int(os.environ.get("SLURM_JOB_NUM_NODES"))
    node_id = int(os.environ.get("SLURM_NODEID"))
    available_gpus = list(os.environ.get('CUDA_VISIBLE_DEVICES').replace(',',""))

    # print local variables
    __l__=locals()
    print({k: __l__[k] for k in filter(lambda x: not x.startswith('__'), dir())})

    torch.cuda.set_device(int(available_gpus[0]))

    dist.init_process_group(
        backend="nccl", # can be mpi, gloo, or nccl
        init_method=init_method,
        world_size=n_nodes,
        rank=node_id,
        timeout=datetime.timedelta(0, 10) # 10s connection timeout
    )
    print('Enter Torch DDP.', flush=True)
    dist.barrier(device_ids=[0]) # wait for other nodes to connect master node
    
    # load the model to CUDA device and do training here...
    # everything is like usual except we wrap our model using: model = DDP(model)
    
    dist.destroy_process_group()
    print('Exit Torch DDP.', flush=True)


if __name__=='__main__':
   fire.Fire(main)
```

#### `sbatch` output
```
$ sbatch sbatch.sh 
Submitted batch job 51384034
$ squeue -u w32zhong
            JOBID     USER              ACCOUNT           NAME  ST  TIME_LEFT NODES CPUS TRES_PER_N MIN_MEM NODELIST (REASON) 
         51384034 w32zhong     def-jimmylin_gpu      sbatch.sh   R       9:57     4    8      gpu:1     32G gra[956,972,974-975] (None) 
$ cat gra956-51384034.out 
+ srun python pytorch-test.py tcp://gra972:8921
{'available_gpus': ['0'], 'init_method': 'tcp://gra972:8921', 'job_id': 51398801, 'local_id': 0, 'n_nodes': 4, 'n_tasks': 4, 'ngpus_per_node': 1, 'node_id': 0, 'proc_id': 0}
Enter Torch DDP.
{'available_gpus': ['0'], 'init_method': 'tcp://gra972:8921', 'job_id': 51398801, 'local_id': 0, 'n_nodes': 4, 'n_tasks': 4, 'ngpus_per_node': 1, 'node_id': 2, 'proc_id': 2}
Enter Torch DDP.
{'available_gpus': ['0'], 'init_method': 'tcp://gra972:8921', 'job_id': 51398801, 'local_id': 0, 'n_nodes': 4, 'n_tasks': 4, 'ngpus_per_node': 1, 'node_id': 3, 'proc_id': 3}
Enter Torch DDP.
{'available_gpus': ['0'], 'init_method': 'tcp://gra972:8921', 'job_id': 51398801, 'local_id': 0, 'n_nodes': 4, 'n_tasks': 4, 'ngpus_per_node': 1, 'node_id': 1, 'proc_id': 1}
Enter Torch DDP.
Exit Torch DDP.
Exit Torch DDP.
Exit Torch DDP.
Exit Torch DDP.
```

### Useful links
* [Slurm job running](https://docs.computecanada.ca/wiki/Running_jobs)
* [Compute Canada for Machine Learning](https://docs.computecanada.ca/wiki/Tutoriel_Apprentissage_machine/en)
* [Compute Canada Wiki for TensorFlow](https://docs.computecanada.ca/wiki/TensorFlow)
* [Compute Canada Wiki for PyTorch](https://docs.computecanada.ca/wiki/PyTorch)
* [Using GPUs with Slurm](https://docs.computecanada.ca/wiki/Using_GPUs_with_Slurm)
* [A simple script to check cluster jobs](https://github.com/t-k-/cc-orchestration/blob/a5f4484e03545ede66fd3165c54d8c6d2dc80596/cc-orchestration.sh)
* [Available software](https://docs.alliancecan.ca/wiki/Available_software)
