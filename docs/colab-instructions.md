# Colab as a GPU instance with full SSH access

For users without local GPU resources, Colab is an available solution. It could be transformed into a GPU instance with full SSH access.

This document refers to this [tutorial](https://vvadithya.medium.com/establishing-an-ssh-connection-to-google-colab-a-step-by-step-guide-6e27a8302eb1).

## Setup sshd
First of all, create a Colab notebook using your Google account and use the GPU runtime mode.
[Ngrok](https://ngrok.com/) is used to make ssh forwarding. Before this, please create a password. Then, let's start `sshd`.
Let's setup and run `sshd`.
```
! apt-get install -qq -o=Dpkg::Use-Pty=0 openssh-server pwgen > /dev/null
password = "yourpassword"
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
You can get authtoken from https://dashboard.ngrok.com/get-started/your-authtoken".
```
! wget -q -c -nc https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-v3-stable-linux-amd64.tgz
! tar -xvzf ngrok-v3-stable-linux-amd64.tgz
import getpass
authtoken = getpass.getpass("Enter your ngrok authtoken: ")
! ./ngrok config add-authtoken $authtoken
get_ipython().system_raw('./ngrok tcp --region=us --log=stdout --log-format=json 22 > ngrok_log.txt &')
```

## Access your server
Now, You can access your server through `ssh` command in your local machine.
The TCP address and port number can be found through the Ngrok interface https://dashboard.ngrok.com/endpoints.
```
ssh root@[tcp_address] -p [port_number]
```

## Time consumption on experiments

These experiments are run under the Colab default setting(T4).

+ [PyGaggle: Baselines on MS MARCO Document Retrieval](https://github.com/castorini/pygaggle/blob/master/docs/experiments-msmarco-document.md): It takes about 4 hours to re-rank each half subset.

+ [PyGaggle: Neural Ranking Baselines on MS MARCO Passage Retrieval](https://github.com/castorini/pygaggle/blob/master/docs/experiments-msmarco-passage.md):
  + Re-Ranking with monoBERT: It takes about 50 mins to re-rank.
  + Re-Ranking with monoT5: It takes about 40 mins to re-rank.
