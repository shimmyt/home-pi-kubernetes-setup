Home Pi K3S Setup
=========

A sample playbook to setup Rancher K3S on a Raspberry Pi

Requirements
------------

This playbook requires at least 2 raspberry pis. Any quad core raspberry pi should be fine.

Role Variables
--------------

All variables are expected to be in group_vars\all. I typically use vault for this as well. Variables expected:
ansible_user: The user all of the playbooks should be run as.
ansible_ssh_private_key_file: Location of your private key for the ansible user
ansible_become_pass: The new password for your ansible_user
pi_password: What you want to set the new pi password to
ansible_password: Should be set to 'raspberry' if this is a new raspbian setup
k3s_users: should be a dictionary with these values:
* name - user name
* password - new password for user
* key - publickey for user
nfs_mounts: For the NFS storage mounts, should be a dictionary
* path
* src

Dependencies
------------

No other ansible roles required.

Using the Playbook
----------------

First it's easiest if you setup a hosts file in a yaml like so:
```yaml
all:
  children:
    cluster:
      children:
        masternode:
          hosts:
            rpi01:
        workernode:
          hosts:
            rpi02:
            rpi03:
```
If you setup new raspbian hosts (recommended) run the playbook like this:
```shell
ansible-playbook config-pikubernetes.yml -i yourinventory.yml --tags firstrun,installdocker,masternode,workernode
```
For subsequent runs you don't need any of the tags.

License
-------

BSD

Author Information
------------------

Shimron Trammell - SRE