# H1 Provision remote Ubuntu node with Ansible example
This repo will provision a remote Ubuntu node with the following.
1. Update apt cache
2. Install a list of packages defined in variable 'packages' within playbook.yml
3. Upgrade current system packages (safely)
4. Allow SSH traffic through ufw
5. Enable ufw and set the default policy to deny
6. Restart the node
7. Wait for the node to be back online
8. Show the system uptime

# Dependencies
1. The remote Ubuntu node must have python installed already, otherwise running this Playbook will fail.
It is suggested to install python using preseed or via cloud-init, otherwise you could also do it manually.

2. You must also have Ansible installed locally.

# Using this Ansible Playbook repo
Before you get started, you must ensure that you have an SSH keypair setup between your client machine
and server. This has been tested using the official latest LTS release of Ubuntu cloud image on Open Stack.
In this case we assume the username is 'ubuntu' and that the user has passwordless sudo.

1. Clone the repo
2. Edit ./hosts and replace the host under [ubuntu_servers] with your own
3. Run 'ansible-playbook -i ./hosts -l ubuntu_servers ./playbook.yml'
4. Watch the automation magic happen

# Notes
The order of tasks within playbook.yml is important.
1. It should update apt cache before we install packages, this is especially true with cloud images
as mostly the image is minified and any local cache is removed.
2. It should add a ufw rule to allow ssh before enabling ufw and setting the default policy to deny,
otherwise you are at best going to see the first run fail, or at worst lock yourself out.
