# Colab as a GPU instance with full SSH access

For users without local GPU resources, Colab is an available solution. It could be transformed into a GPU instance with full SSH access.

This document refers to this [tutorial](https://imadelhanafi.com/posts/google_colal_server/).

## Setup sshd
First of all, create a Colab notebook using your Google account and use the GPU runtime mode.
[Ngrok](https://ngrok.com/) is used to make ssh forwarding. Before this, please create a password. Then, let's start `sshd`.
Let's setup and run `sshd`.
```
! apt-get install -qq -o=Dpkg::Use-Pty=0 openssh-server pwgen > /dev/null
! echo root:$password | chpasswd
! mkdir -p /var/run/sshd
! echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
! echo "PasswordAuthentication yes" >> /etc/ssh/sshd_config
! echo "ClientAliveInterval 30" >> /etc/ssh/sshd_config
! echo "ClientAliveCountMax 5" >> /etc/ssh/sshd_config
get_ipython().system_raw('/usr/sbin/sshd -D &')
```

## Setup Ngrok
After that, we can install and run Ngrok, which creates a TCP tunnel.
You can get authtoken from https://dashboard.ngrok.com/auth".
```
! wget -q -c -nc https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip
! unzip -qq -n ngrok-stable-linux-amd64.zip
import getpass
authtoken = getpass.getpass()
get_ipython().system_raw('./ngrok authtoken $authtoken && ./ngrok tcp 22 &')
```

## Access your server
Now, You can access your server through `ssh` command in your local machine.
The TCP address and port number can be found through the Ngrok interface https://dashboard.ngrok.com/status.
```
ssh root@[tcp_address] -p [port_number]
```

## Time consumption on experiments

These experiments are run under the Colab default setting.

+ [PyGaggle: Baselines on MS MARCO Document Retrieval](https://github.com/castorini/pygaggle/blob/master/docs/experiments-msmarco-document.md): It takes about 8 hours to re-rank each half subset.

+ [PyGaggle: Neural Ranking Baselines on MS MARCO Passage Retrieval](https://github.com/castorini/pygaggle/blob/master/docs/experiments-msmarco-passage.md):
  + Re-Ranking with monoBERT: It takes about 50 mins to re-rank.
  + Re-Ranking with monoT5: It takes about 70 mins to re-rank.
