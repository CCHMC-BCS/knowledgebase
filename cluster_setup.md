# Cluster Setup

## Requesting a cluster account

## Making an SSH key on the cluster

I suggest making the SSH key on the cluster. You can do this by logging into the cluster and running
```
ssh-keygen
```
On the command line. Use the default location for the key: .ssh/id_rsa. I usually use an empty passphrase. You'll
download this key to your local machine, linking the computers together, in one of the next two sections
depending on your operating system.

Then you need to tell the server that people with this key are authorized to connect using it:
```
cat ~/.ssh/id_rsa.pub >>~/.ssh/authorized_keys
```

## Setting up SSH on Windows for use by VC Code ##

You'll need install the OpenSSH client so that you can connect to the cluster. Follow the instructions here:

https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse#installing-openssh-from-the-settings-ui-on-windows-server-2019-or-windows-10-1809

Then open the Command Prompt (hit the windows key, type "cmd", and hit enter) and the make the .ssh directory:
```
mkdir .ssh
```
Then, start Visual Studio Code to edit the SSH config file
```
cd
cd .ssh
code config
```
And add the following lines, making sure to change *both* instances YOURUSERNAME to your cluster username, before saving the file:
```
Host *
    ServerAliveInterval 60
Host ssh cchmc
    HostName ssh.research.cchmc.org
    User YOURUSERNAME
Host bmiclusterp bmiclusterp2 cluster
    Hostname bmiclusterp.chmcres.cchmc.org
    User YOURUSERNAME
    ProxyCommand C:\Windows\System32\OpenSSH\ssh.exe -q ssh nc -w 180 %h %p
```
(the extra hope through ssh.research.cchmc.org means you'll be able to connect without having to use the VPN) then you
should be able to download the private key by running (in the .ssh directory):
```
scp cluster:.ssh/id_rsa .
```
Once it's downloaded, you should be able to run
```
ssh cluster
```
without having to enter your password.

## Setting up SSH on MacOSX/Linux for use by VC Code

Linux and MacOSX already have SSH installed so you only have to modify the config file. Edit ~/.ssh/config
(note this file might not already exists but that's ok) and add the following lines, making sure to change
*both* instances YOURUSERNAME to your cluster username, before saving the file:
```
Host *
    ServerAliveInterval 60
Host ssh cchmc
    HostName ssh.research.cchmc.org
    User YOURUSERNAME
Host bmiclusterp bmiclusterp2 cluster
    Hostname bmiclusterp.chmcres.cchmc.org
    User YOURUSERNAME
    ProxyCommand ssh -q ssh nc -w 180 %h %p
```
(the extra hope through ssh.research.cchmc.org means you'll be able to connect without having to use the VPN) then you
should be able to download the private key by running (in the .ssh directory):
```
scp cluster:.ssh/id_rsa ~/.ssh/
```
Once it's downloaded, you should be able to run
```
ssh cluster
```
without having to enter your password.