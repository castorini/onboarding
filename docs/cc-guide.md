# Compute Canada

We have resource allocations from Compute Canada for most of [their supercomputers](https://www.computecanada.ca/research-portal/accessing-resources/available-resources/). To request access, create an account on their [website](http://ccdb.computecanada.ca) and follow [these instructions](https://www.computecanada.ca/research-portal/account-management/apply-for-an-account/). Jimmy's CCRI is `ssi-230-01`.

The three GPU-enabled clusters that we mainly use are as follows:

### Graham
```
Hostname: graham.computecanada.ca
Maximum specifications: 2x P100 GPUs **OR** 8x V100 GPUs, 64GB RAM.
```

### Cedar
```
Hostname: cedar.computecanada.ca
Maximum specifications: 4x P100 GPUs, 64GB RAM.
```

### Beluga
```
Hostname: beluga.computecanada.ca
Maximum specifications: 4x V100 GPUs (16GB version), 64-186GB RAM.
```

From the login nodes, you submit jobs using SLURM, a standard resource scheduler for computing clusters.
After some time, the tasks begin running on the internal computing nodes with the desired configuration -- see [the official documentation](https://docs.computecanada.ca/wiki/Running_jobs) for an in-depth guide.

Note that you are subject to [disk quotas at the individual and group level](https://docs.computecanada.ca/wiki/Storage_and_file_management). One common workflow is to install Anaconda to your home directory for local Python package management, then save all data and models to the temporary `~/scratch` directory, which has a much higher disk quota.
