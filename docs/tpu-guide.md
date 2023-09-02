# Guide to TPUs
The official Google documentation on Cloud TPU covers a ton of details, this guide aims to offer a very summarized guidance.

## Free trial application
As of the time of writing, Google offers a free TPU trial for 31 days.
The typical quota includes
* 5 on-demand Cloud TPU v2-8
* 100 preemptible Cloud TPU v2-8 devices
* 5 on-demand Cloud TPU v3-8 devices

> A preemptible VM is an instance that you can create and run at a much lower price than normal instances. However, Compute Engine might stop (preempt) these instances if it requires access to those resources for other tasks. Preemptible instances are excess Compute Engine capacity, so their availability varies with usage.
> (https://cloud.google.com/compute/docs/instances/preemptible)

You can start if you are a student or researcher.
Once you have received an email response, create a Google cloud project, turn on Cloud TPU API on your Google cloud console and submit your project **number** (not project name) to an online form provided in the email.

Then you will receive another email regarding specific quota and cloud node region/zone restrictions offered by Google cloud.
Notice that this free 31-day trial is only available for the zones listed in the email.
Be sure to create in those zones to avoid being charged. (use `gcloud compute operations list` to view instances you have created, it is also a useful command to double-check if the zone matches free TPU restriction)

## Quickstart
Follow [gcloud SDK installation](https://cloud.google.com/sdk/docs/install) and [this Quickstart](https://cloud.google.com/tpu/docs/pytorch-quickstart-tpu-vm) to install command-line interface `gcloud` and create your ComputeEngine and TPU cloud instances.
The ComputeEngine is necessary to access TPU since the TPU node does not expose public addresses to be accessed.
While your Cloud TPUs are free, you’ll still be charged for the other cloud services you use.

TPU specs can be found here: https://cloud.google.com/tpu/docs/tpus

If you are a student, you can [redeem](https://cloud.google.com/billing/docs/how-to/edu-grants#redeem) USD 300 credits education grants, this introductory credit may help you offset these costs.
To minimize cost while compensating the large memory need to do data-parallel training for a large model, you may change the suggested ComputeEngine configuration (i.e., `n1-standard-16`) to high-memory instances such as `n1-highmem-16`.
In some scenarios, especially during pretraining, when multiple copies of a large dataset need to be loaded into main memory, you can increase memory very cheaply by creating a swap partition:
```sh
sudo fallocate -l 20G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```

For PyTorch users, once you have set up and connected to the TPU node, follow [Pytorch XLA tutorial](https://pytorch.org/xla/release/1.9/index.html) to rewrite your program so it can adapt to the Pytorch XLA compiler and run on TPU device.
For a single XLA core, a minimal change of code is good-to-go:

```python
import torch_xla.core.xla_model as xm

device = xm.xla_device()

# at training ...
  optimizer.step()
  xm.mark_step()
```

To programally print the memory of one TPU core,
```python
import torch_xla
import torch_xla.core.xla_model as xm
dev = xm.xla_device()
dev_info = torch_xla.core.xla_model.get_memory_info(dev)
total_mem = dev_info['kb_total'] / 1000
print('TPU memory: ', total_mem)
```

In TPU hardware, a chip consists of two cores, in a `V2-8` or `V3-8` hardware setting, each TPU unit contains 4 chips, so in total 8 cores per TPU unit.
