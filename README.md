# Ansible Tower and Ansible Automation Platform Installer Playbook

## Purpose:

An easy way to install Ansible Tower or Ansible Automation Platform for testing.

## Usage:

There are only a few required variables to allow the playbook to run properly:

### Node Types:

primary_node (The primary node for Tower/AAP)

secondary_node (The secondary node for Tower/AAP when running in a cluster. Needs primary_node defined)

tertiary_node (The third node for Tower/AAP when running in a multi-node cluster. Needs primary_node and secondary_node defined.)

db_node (The database node when using an external database)

ah_node (The Automation Hub node)

### Version:
ver (The version of Tower or AAP to install. Example: 2.2.0-6.1)

### Red Hat Subscription Variables:
There are two ways to define these. As an extra var or environment variable.

- Extra Vars:

subscription_username

subscription_password

- Environment Vars:

RHSUBUN (RH Account Username)

RHSUBPW (RH Account Password)


### Optional Variables:

1. install_type:

This should automatically be able to be set based on the version provided. Accepted values are "aap", "AAP", "tower", and "Tower" if it does need to be defined.

2. Registry Variables:

By default the registry_username and registry_password values will be set using the subscription_ values. Can also be defined as extra vars or environment variables.

- Extra Vars:

registry_username

registry_password

- Environment Vars:

RHREGUN (RH Registry Username)

RHREGPW (RH Registry Password)

## Example Install Scenarios:

### Single Node Tower v3.8.6-2:
ansible-playbook tower-aap-install.yaml -e "primary_node=tower386.fqdn ver=3.8.6-2"

### Single Node AAP 2.2.0-7:
ansible-playbook tower-aap-install.yaml -e "primary_node=aap220.fqdn ver=2.2.0-7"

### 3 Node Cluster Tower 3.8.6-2:
ansible-playbook tower-aap-install.yaml -e "primary_node=tower386n1.fqdn secondary_node=tower386n2.fqdn tertiary_node=tower386n3.fqdn db_node=db386.fqdn ver=3.8.6-2"

### Single Node Automation Hub 1.2.7-2:
ansible-playbook tower-aap-install.yaml -e "ah_node=ah127.fqdn ver=1.2.7-2"

### Single Node Automation Hub w/External DB 1.2.7-2:
ansible-playbook tower-aap-install.yaml -e "ah_node=ah127.fqdn db_node=db127.fqdn ver=1.2.7-2"

## Install Notes:

1. There are a few setup steps specific to my environment. I tried to note them down with comments in the playbook.
2. I downloaded the Ansible Automation Platform Installer Bundles locally. Unsure of how to get them otherwise, but the source path (/mnt/aap/aap-bundles) in the playbook may need to be modified to reflect any custom paths used.
3. I didn't have a RHEL image with an ansible version low enough to install Tower 3.3.0-1 or Tower 3.4.0-1. I believe I ran into ansible related errors when trying to install these on RHEL 7.4 with ansible core 2.12. I believe they should work based on the inventory though. The earliest version of Tower I tested for was 3.5.0-1 on RHEL 7.4.
4. Any errors, ideas, or recommendations would be greatly appreciated!
