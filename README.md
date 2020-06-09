# vmware-automation

This repository contains example ansible playbooks for creating VMWare guests.
The main playbooks are

- create-from-template.yml
- destroy-vm.yml

## create-from-template playbook

This playbook uses the create-vm role to create roles in VMWare. All required variables are in the _inventory/_ folder
in the same repository.

## delete-vm playbook

Deletes a VM from vCenter. The VM name is specified via the _vmname_ parameter. For example

```bash
$ ansible-playbook -i inventory delete-vm.yml -e vmname=testvm01
```

will forcefully delete the VM _testvm01_ from the vcenter. 

## create-vm role

The create vm role requires the following parameters

- vcenter_hostname: The hostname of the vCenter to connect to 
- vcenter_username: Username for authentication 
- vcenter_password: Should contain the vault encrypted password for _vcenter_username_
_ vcenter_datacenter: The vSphere datacenter to use for creating VM's
- vcenter_folder: The folder where to create the VM's
- vcenter_cluster_name: vSphere cluster where VM's should be created
- vcenter_template: Name of the template to be used for cloning VM's
- vcenter_datastore: Name of the datastore to use for VM's
- vcenter_validate_certs: If SSL certificates should be validated

Here is an example play for creating two test vms

```ansible
- name: Create a VM from a template
  hosts: localhost
  gather_facts: no
  collections:
    - community.vmware
  become: no
  roles:
    - role: create-vm
      vars:
        vms:
          - name: testvm01
            ipaddr: 10.0.0.142
            netmask: 255.255.255.0
            dns_domain: lan.stderr.at
            dns_servers:
              - 10.0.0.254
              - 8.8.8.8
            dns_suffix:
              - lan.stderr.at
              - stderr.at
          - name: testvm02
            ipaddr: 10.0.0.143
            netmask: 255.255.255.0
            memory_mb: 1024
            num_cpus: 1
            disk_size_gb: 20
            dns_domain: lan.stderr.at
            dns_servers:
              - 10.0.0.254
            dns_suffix:
              - lan.stderr.at
              - stderr.at
```

## Dependencies

The following python modules must be installed

- [pyvmomi](https://github.com/vmware/pyvmomi) in a version matching your vSphere version
- [vsphere-automation-sdk-python](https://github.com/vmware/vsphere-automation-sdk-python.git) if you want to try using vmwware_vm_inventory


