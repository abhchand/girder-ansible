Ansible playbook for for [girder](https://gitlab.com/girder/girder)

# Quick-Start

Ensure `ansible-playbook` is installed locally,

Create `hosts.ini` with the following and replace `XXXX` with the server IP address or Hostname. You can use a local VM for development tiers - see below section for instructions.

```
[prod]
XXXXX

[dev]
XXXXX

[all:children]
prod
dev
```

Create a remote user named `ansible` on the remote server and add them to the sudo-ers group

```
adduser ansible
gpasswd -a ansible sudo
```

Use the `run` binstub with a specified tier.

```
bin/run dev
bin/run prod
```


# Virtual Machine Setup (Development)

You can set up a local virtual host to perform a trial run of the ansible playbook for development

Create a new server and map the following ports
  - `ssh` - 2222 (host) -> 22 (virtual machine)
  - `ssh` - 8080 (host) -> 80 (virtual machine)


Add the following to your `hosts.ini` file

```
[dev:vars]
vm=1
ansible_port=2222
```

Set up `ssh` on the VM
```
# Install open-ssh
sudo apt install openssh-server

# Start SSH service
sudo service ssh start

# Verify it is running
sudo lsof -i -n -P | grep ssh
```

Add an entry for this server in your `/etc/hosts` file

```
sudo vi /etc/hosts

# Add below line. Hostname can be anything.
127.0.0.1       dev-1001.example.co
```

You can now access the virtual machine from your local host

```
ssh -p 2222 dev-1001.example.co
```
