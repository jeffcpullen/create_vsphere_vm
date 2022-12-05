Role Name
=========

This role deploys virtual machines to a vsphere environment from templates in a content library.

Requirements
------------

A content library must exist in the vmware environment to pull "golden images" from. This blog provides an overview of the creation of a content library <https://blogs.vmware.com/vsphere/2020/01/creating-and-using-content-library.html>

Role Variables
--------------

| Variable | Default | Description |
| -------------- | --------- | ---------------------------------------------- |
| create_vsphere_vm_datacenter | Homelab DC | vSphere Datacenter name |
| create_vsphere_vm_cluster | Gen2 | Datastore to create VM on |
| create_vsphere_vm_folder | unset | Folder to create the VM in (Optional) |
| create_vsphere_vm_library | golden_images_library | Name of the content library containing the template |
| vcenter_validate_certs | false | Whether to validate TLS certs |
| create_vsphere_vm_name | TESTVM | Name of the VM to create |
| create_vsphere_vm_state | deploy | Action to apply against VMware API. NOTE: The vmware_rest module is not idempotent. Use 'deploy' to create, and 'present' after created |
| create_vsphere_vm_template | rhel8-base | Source template name to use for deployment |
| create_vsphere_vm_memory_size_mib | 1024 | Number of MB of RAM to assign to VM |
| create_vsphere_vm_cpu_cores | 1 | Number of cores to assign to VM |
| create_vsphere_vm_start_vm | true | Start the VM after creation |
| create_vsphere_vm_require_template | false | Instructs the role to use an ova/ovf files rather than a template |

Dependencies
------------

Python module aiohttp >= 3.8

```yaml
collections:
  - name: vmware.vmware_rest
    version: 2.2.0
  - name: community.general
    version: 6.0.1
```

Example Playbook
----------------

Example playbook to create a virtual machine named snaap.lab.homelab.work
with 8Gb of RAM and 2 CPU cores based on the rhel8-base-template

```yaml
  - name: Sample playbook to execute create_vsphere_vm role
    hosts: localhost
    connection: local
    tasks:

    - name: Call create_vsphere_vm role
      vars:
        create_vsphere_vm_template: 'rhel8-base-template'
        create_vsphere_vm_name: 'snaap.lab.homelab.work'
        create_vsphere_vm_memory_size_mib: 8192
        create_vsphere_vm_cpu_cores: 2
      ansible.builtin.include_role:
        name: create_vsphere_vm
```

License
-------

MIT

Author Information
------------------

Jeff Pullen
Red Hat
