### Red Hat Openstack Platform 16.2 Ansible Database Backup Automation

### Disclaimer
The playbooks in this repository are free to use and come without warranty or
support of Red Hat or the repository owner.

### About this repository
This repository contains Ansible playbooks used to perform Undercloud and 
Overcloud database backups without instsalling ReaR in Red Hat OpenStack Platform 16.x environments. 

Red Hat OpenStack Platform 16.x recommends the use of the Relax and Recover tool
for the purpose of performing image based backups of Undercloud and Overcloud
controllers. There are certain use cases where performing image based backups of
entire Overcloud controller nodes is not desirable, and you are interested in a
way to perform database only backups of the Undercloud and Overcloud databases.

The playbooks introduced in this repository allows for the backup of a
containerized Undercloud and Overcloud nodes. 


### Requisites
- Ansible >= 2.9
- Passwordless ssh authentication from the Ansible execution host to the Undercloud node and Overcloud controllers.
- Access to a functioning Undercloud and Overcloud. This is after all our target backup environments.
- NFS server with sufficient storage to accommodate the Undercloud and Overcloud backups. This server will act as the remote repository server where all the database backups will be stored outside of the Undercloud and Overcloud.
- NFS server connectivity from the Undercloud and Overcloud hosts to the remote backup host. This is required as the playbooks will temporarily mount an NFS filesystem and will write the contents of the Undercloud and Overcloud database backups.

### Repository structure
```
├── inventory
├── overcloud_db_backup.yml
├── README.md
├── tasks
│   ├── overcloud_database_backup.yml
│   └── undercloud_database_backup.yml
└── undercloud_db_backup.yml
```

### Usage

#### Example: Backing up the Undercloud
`ansible-playbook -i inventory undercloud_db_backup.yml`

#### Example: Backing up the Overcloud
`ansible-playbook -i inventory overcloud_db_backup.yml`


#### Restoring the Undercloud 
To restore the Undercloud node database manually which is the intended use case
for these database backup playbooks, follow the procedure documented in [Section 3.5:
Restoring the undercloud node database
manually](https://access.redhat.com/documentation/en-us/red_hat_openstack_platform/16.2/html/backing_up_and_restoring_the_undercloud_and_control_plane_nodes/assembly_restoring-the-undercloud-and-control-plane-nodes_br-undercloud-ctlplane#proc_restoring-the-undercloud-node-database-manually_restore-undercloud-ctlplane).

#### Restoring the Overcloud
To restore the Overcloud Galera cluster manually which is the intended use case
for these database backup playbooks follow the procedure documented in [Section 3.4: Restoring
the Galera cluster manually. In the Red Hat OpenStack Platform 16.2
documentation](https://access.redhat.com/documentation/en-us/red_hat_openstack_platform/16.2/html/backing_up_and_restoring_the_undercloud_and_control_plane_nodes/assembly_restoring-the-undercloud-and-control-plane-nodes_br-undercloud-ctlplane#proc_restoring-galera-cluster-manually_restore-undercloud-ctlplane).

### How do these playbooks work ? 
undercloud_db_backup.yml is a top level playbook with a simple set of variables.
The tasks are imported from the undercloud_database_backup.yml file in the tasks
directory. This is what occurs on the Undercloud once this
playbook is invoked:
- NFS client mount directory is created in the Undercloud
- NFS share is mounted on the Undercloud
- Run-once backup directory is created in the Undercloud. This is where the
  database files are stored
- MySQL root password is retrieved from hieradata in the Undercloud. This
  prevents from needing to declare passwords as playbook variables for later
use.
- The mysqldump command is executed inside the database container in the
  Undercloud. These database dump file is compressed and stored in today's
backup directory.


#### Undercloud Backups


#### Overcloud Backups
