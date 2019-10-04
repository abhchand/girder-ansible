Ansible playbook for for Girder

# Local Setup

Ensure ansible is installed locally,

Create `hosts.ini` with the following and rreplace `XXXX` with the server IP address or Hostname

```
[prod]
XXXXX

[dev]
XXXXX

[dev:vars]
vm=1
ansible_port=2222

[all:children]
prod
dev
```

# Virtual Machine Setup (Optional)

You can set up a local virtual host to perform a trial run of the ansible playbook.

Below instructions assume a debian operating system such as Ubuntu

Create a new server and map the following ports
  - `ssh` - 2222 (host) -> 22 (virtual machine)
  - `ssh` - 8080 (host) -> 80 (virtual machine)

Perform the OS install

```
# Install open-ssh
sudo apt install openssh-server

# Start SSH service
sudo service ssh start

# Verify it is running
sudo lsof -i -n -P | grep ssh

# Create a personal user for yourself, with sudo permissions
adduser blahblah
gpasswd -a blahblah sudo
```

Add an entry for this server in your `/etc/hosts` file

```
sudo vi /etc/hosts

# Add below line. Hostname can be anything.
127.0.0.1       dev-1001.example.co
```

You can now access the virtual machine from your local host

```
ssh -p 2222 blahblah@dev-1001.example.co
```

Continue with the remote host setup instructions below

After setting up the remote (virtual) host, be sure to

1. Save the state of your virtual machine so you can rollback for easy testing
2. Save the root, personal user, and ansible user login credentials somewhere


# Server Setup

Create a remote user named `ansible` on the remote server and add them to the sudo-ers group

```
adduser ansible
gpasswd -a ansible sudo
```

Verify user was created:

```
> cat /etc/passwd | grep ansible
ansible:x:1000:1000:,,,:/home/ansible:/bin/bash
```

# Deploy

Use the `run` binstub with a specified tier.

```
bin/run dev
bin/run prod
```
