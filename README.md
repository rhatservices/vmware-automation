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
