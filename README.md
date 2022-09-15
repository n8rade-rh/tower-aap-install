<p align="center">
  <a href="#purpose">Purpose</a> •
  <a href="#how-to-use">How To Use</a> •
  <a href="#required-variables">Required Variables</a> •
  <a href="#optional-variables">Optional Variables</a> •
  <a href="#install-notes">Install Notes</a> •
</p>

## Purpose

An easy way to install Ansible Tower or Ansible Automation Platform for testing and troubleshooting.

## How To Use

### Single Node Tower v3.8.6-2:
```bash
ansible-playbook tower-aap-install.yaml -e "primary_node=tower386.fqdn ver=3.8.6-2"
```

### Single Node AAP 2.2.0-7:
```bash
ansible-playbook tower-aap-install.yaml -e "primary_node=aap220.fqdn ver=2.2.0-7"
```

### 3 Node Cluster Tower 3.8.6-2:
```bash
ansible-playbook tower-aap-install.yaml -e "primary_node=tower386n1.fqdn secondary_node=tower386n2.fqdn tertiary_node=tower386n3.fqdn db_node=db386.fqdn ver=3.8.6-2"
```

### Single Node Automation Hub 1.2.7-2:
```bash
ansible-playbook tower-aap-install.yaml -e "ah_node=ah127.fqdn ver=1.2.7-2"
```

### Single Node Automation Hub w/External DB 1.2.7-2:
```bash
ansible-playbook tower-aap-install.yaml -e "ah_node=ah127.fqdn db_node=db127.fqdn ver=1.2.7-2"
```

> **Note**
> The above examples assume that the subscription_username and subscription_password are defined via environment variables. They can also be defined as extra vars in the above commands. Please read more about the required variables.


## Required Variables

### Node Types: 
(One at a minimum needs to be defined)

```bash
primary_node (The primary node for Tower/AAP)

secondary_node (The secondary node for Tower/AAP when running in a cluster. Needs primary_node defined)

tertiary_node (The third node for Tower/AAP when running in a multi-node cluster. Needs primary_node and secondary_node defined.)

db_node (The database node when using an external database)

ah_node (The Automation Hub node)
```

### Version:

```bash
ver (The version of Tower or AAP to install. Example: 2.2.0-6.1)
```

### Red Hat Subscription Variables:
There are two ways to define these. As an extra var or environment variable.

Extra Vars:
```bash
subscription_username
subscription_password
```

Environment Vars:
```bash
RHSUBUN (RH Account Username)
RHSUBPW (RH Account Password)
```

## Optional Variables

### install_type:

This should automatically be able to be set based on the version provided. Accepted values are "aap", "AAP", "tower", and "Tower" if it does need to be defined.

### Registry Variables:

By default the registry_username and registry_password values will be set using the subscription values. Can also be defined as extra vars or environment variables.

Extra Vars:
```bash
registry_username
registry_password
```

Environment Vars:
```bash
RHREGUN (RH Registry Username)
RHREGPW (RH Registry Password)
```

### Password Variables

admin_password and pg_password variables. Extra or environment variables. If not set, defaults to "redhat".

Extra Vars:
```bash
admin_password
pg_password
```

Environment Vars:
```bash
ADMINPW
PGPW
```

### debug_inventory
Simply needs to be defined and can be any value. Will skip running setup.sh and all steps after (pinging API's) for inventory troubleshooting purposes.

Example:
```bash
ansible-playbook tower-aap-install.yaml -e "primary_node=aap220.fqdn ver=2.2.0-7 debug_inventory="
```

### delete_old_files
When running successive runs with this playbook, this can be defined (to any value) to allow for the old renamed folder to be deleted if it exists on the main node. Mainly only useful when using the script multiple times on the same machine. Eventually the folder rename operation fails due to the folder already existing.

Example:
```bash
ansible-playbook tower-aap-install.yaml -e "primary_node=aap220.fqdn ver=2.2.0-7 delete_old_files="
```

## Other Notes

1. There are a few setup steps I needed to define that were specific to my environment. They have been mostly removed from this version, but there is a setup section that can run any setup pre-reqs on all nodes if running one of the multi-node installs.
2. I downloaded the Ansible Automation Platform Installer Bundles locally. Unsure of how to get them otherwise, but the source path (/mnt/aap/aap-bundles) in the playbook may need to be modified to reflect any custom paths used.
3. I didn't have a RHEL image with an ansible version low enough to install Tower 3.3.0-1 or Tower 3.4.0-1. I believe I ran into ansible related errors when trying to install these on RHEL 7.4 with ansible core 2.12. I think they should work based on the inventory though. The earliest version of Tower I tested for was 3.5.0-1 on RHEL 7.4.
4. Any errors, ideas, or recommendations would be greatly appreciated!
