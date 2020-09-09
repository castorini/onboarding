# Colab as a GPU instance with full SSH access

For users without local GPU resources, Colab is an available solution. It could be transformed into a GPU instance with full SSH access.

First of all, create a Colab notebook using your Google account and use the GPU runtime mode.
[Ngrok](https://ngrok.com/) is used to make ssh forwarding. Before this, please create a password. Then, let's start `sshd`.
Let's setup and run `sshd`.
```
! apt-get install -qq -o=Dpkg::Use-Pty=0 openssh-server pwgen > /dev/null
! echo root:$password | chpasswd
! mkdir -p /var/run/sshd
! echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
! echo "PasswordAuthentication yes" >> /etc/ssh/sshd_config
get_ipython().system_raw('/usr/sbin/sshd -D &')
```
After that, we can install and run Ngrok, which creates a TCP tunnel.
You can get authtoken from https://dashboard.ngrok.com/auth".
```
! wget -q -c -nc https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip
! unzip -qq -n ngrok-stable-linux-amd64.zip
import getpass
authtoken = getpass.getpass()
get_ipython().system_raw('./ngrok authtoken $authtoken && ./ngrok tcp 22 &')
```
Now, You can access your server through `ssh` command in your local machine.
The Port number can be found through the Ngrok interface https://dashboard.ngrok.com/status.
```
ssh root@0.tcp.ngrok.io -p [port_number]
```
