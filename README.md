### Red Hat Openstack Platform 16.2 Ansible Driven Database Backup Automation

### Disclaimer
The playbooks in this repository are free to use and come without warranty or
support of Red Hat or the repository owner.

### About this repository
This repository contains Ansible playbooks used to perform Undercloud and Overcloud database backups outside of ReAr.


### Requisites
- Ansible >= 2.9
- Passwordless ssh authentication from the Ansible execution host to the Undercloud node and Overcloud controllers.
- Access to a functioning Undercloud and Overcloud. This is after all our target backup environments.
- NFS server with sufficient storage to accommodate the Undercloud and Overcloud backups. This server will act as the remote backup host.
- NFS server connectivity from the Undercloud and Overcloud hosts to the remote backup host. This is required as the playbooks will temporarily mount an NFS filesystem and will write the contents of the Undercloud and Overcloud database backups.

For additional information refer to the [Introduction to Undercloud and Control
Plane Backup and Restore](https://access.redhat.com/documentation/en-us/red_hat_openstack_platform/16.1/html/undercloud_and_control_plane_back_up_and_restore/introduction-to-undercloud-and-control-plane-back-up-and-restore_osp-ctlplane-br) documentation.

### Usage

#### Example: Backing up the Undercloud
`ansible-playbook -i inventory undercloud_db_backup.yml`

#### Example: Backing up the Overcloud
`ansible-playbook -i inventory overcloud_db_backup.yml`

#### Restoring the Undercloud and Overcloud
To restore the Undercloud or Overcloud controllers follow the procedures as documented in [Chapter 5: Restoring the Undercloud and Control Plane Nodes](https://access.redhat.com/documentation/en-us/red_hat_openstack_platform/16.1/html/undercloud_and_control_plane_back_up_and_restore/restoring-the-undercloud-and-control-plane-nodes_osp-ctlplane-br). 



