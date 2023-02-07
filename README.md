Collection of scripts and Ansible playbooks to setup vanilla Ubuntu hosts for:

 - SSH Pubkey auth on each host, removing password auth
 - passwordless sudo
 - Install necessary packages
 - Install Docker engine
 - Pull DVWM docker image from DockerHub
 - Create and start specified number of DVWM containers on specified ports

There are also scripts to install Ansible on an Ansible management node.

Here are descriptions for each directory:

ansible_setup - location of Ansible install scripts to get Ansible setup on new Ansible management node
docker - collection of Ansible playbooks to install Docker engine and manage DVWM containers on hosts
host_setup - Ansible playbook which sets up vanilla Ubuntu hosts to get them "Ansible friendly"
manage_hosts - Playbooks to enable/disable password auth for SSHD on each host (not really needed)

Step-by-step guide to prepare, install Docker engine and run DVWM containers on brand-new, vanilla Ubutnu hosts:

On Ansible management node, clone git repository:

git clone https://github.com/kb3ybe/ansible-docker.git

1. cd host_setup
2. Copy contents of hosts file into /etc/ansible/hosts if desired.
3. Edit initial_host_setup.yml and specify admin_username as the user created during Ubuntu installation.
4. ansible-playbook ./initial_host_setup.yml -k -K

add -u [admin user] to command above if host admin user is different from current user on ansible node

5. Supply SSH password for user above.
6. Supply sudo password for user above if different from SSH password.
7. cd ../docker
8. ansible-playbook ./install_docker_engine_onto_hosts.yml
9. Edit create_and_start_dvwa_containers_on_hosts.yml to specify container_count and container_ports.
10. ansible-playbook ./create_and_start_dvwa_containers_on_hosts.yml
11. ssh to hosts and do a "sudo docker ps" to verify containers were created and ports are correct.

At this point, you will have a specified number of DVWA containers running at the specified ports on each host.  You can verify operation by pointing a browser at each host and port combination. 
