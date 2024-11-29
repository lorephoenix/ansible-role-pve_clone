Ansible Role: pve_clone
------------

This Ansible role facilitates cloning and managing virtual machines (VMs) on a Proxmox Virtual Environment (PVE) cluster. It supports tasks such as creating VMs from templates, updating VM configurations, starting VMs, and removing VMs.

```
    git clone https://github.com/lorephoenix/ansible-role-pve_clone pve_clone
```

Requirements
------------

This role makes use of the community.general collection, which is part of the Ansible package and includes many modules and plugins supported by Ansible community which are not part of more specialized community collections.

To install the collection:
```
    ansible-galaxy collection install -r requirements.yml
```

Role Variables
--------------

The following variables can be customized to suit your environment. Default values are defined in `defaults/main.yml`

| Variable | Value | Data Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `pve_full_copy`     | `true`               | Boolean               | Optional  | Whether to create a full copy of the VM.         |
| `pve_host`          | `proxmox.example.com`| String                | Mandatory | Proxmox server hostname or IP address.           |
| `pve_node`          | `pve`                | String                | Mandatory | The target Proxmox node for VM creation.         |
| `pve_pool`          | `test`               | String                | Optional  | Proxmox resource pool name                       |
| `pve_port`          | `8006`               | Integer               | Optional  | Proxmox API port.                                |
| `pve_state`         | `present`            | String                | Mandatory | Desired state of the VM (`present` or `absent`). |
| `pve_templateid`    | `100`                | Integer               | Mandatory | The ID of the Proxmox VM template to clone.      |
| `pve_timeout`       | `500`                | Integer               | Optional  | Timeout for API requests in seconds.             |
| `pve_tokenid`       | `root@pam!mytokenid` | String                | Mandatory | API token ID for authentication.                 |
| `pve_token_secret`  | `*******`            | String                | Mandatory | API secret token                                 |
| `pve_validate_certs`| `false`              | Boolean               | Optional  | Verify SSL certificate if using HTTPS.           |
| `new_vm`            | See example below    | List of dictionaries  | Mandatory | List of VM configurations for cloning.           |

### `new_vm` Example:

```yaml
new_vm:
  - name: "clone-vm-01"
    format: raw
    storage: "local-lvm"
```

The new_vm variable allows you to define the configuration for new virtual machines to be created. Below is the structure and explanation of each sub-variable:
| Variable | Example Value | Data Type | Description |
| :--- | :--- | :--- | :---|
| `new_vm`                   | -                                      | List         | A list of virtual machine configurations. Each item represents a VM.       |
| `new_vm.name`              | `"clone-vm-1"`                         | String       | The name of the new VM.                                                    |
| `new_vm.config`            | -                                      | Dictionary   | VM configuration parameters.                                               |
| `new_vm.config.ciupgrade`  | `true`                                 | Boolean      | Whether to enable cloud-init upgrades.                                     |
| `new_vm.config.ciuser`     | `"root"`                               | String       | Default user for cloud-init.                                               |
| `new_vm.config.cipassword` | `"root"`                               | String       | Default password for cloud-init.                                           |
| `new_vm.config.cores`      | `4`                                    | Integer      | Number of CPU cores allocated to the VM.                                   |
| `new_vm.config.description`| `"Clone Virtual machine 1"`            | String       | Description of the VM.                                                     |
| `new_vm.config.disk_id`    | `"virtio0"`                            | String       | The disk identifier.                                                       |
| `new_vm.config.disk_size`  | `"+5G"`                                | String       | Additional disk size to allocate.                                          |
| `new_vm.config.ipconfig.ipconfig0` | `'ip=192.168.1.101/24,gw=192.168.1.1'` | String | Network configuration for the VM.                                          |
| `new_vm.config.kvm`        | `false`                                | Boolean      | Whether to enable or disable KVM hardware virtualization.                  |
| `new_vm.config.memory`     | `2048`                                 | Integer      | Amount of RAM (in MB) allocated to the VM.                                 |
| `new_vm.config.nameservers`| `['192.168.1.1']`                      | List         | List of nameservers for the VM.                                            |
| `new_vm.config.net.net0`   | `'virtio,bridge=vmbr0'`                | String       | Network adapter configuration (type and bridge).                           |
| `new_vm.config.onboot`     | `true`                                 | Boolean      | Whether to start the VM on Proxmox node boot.                              |
| `new_vm.config.searchdomains`| `"example.com"`                      | String       | DNS search domain for the VM.                                              |
| `new_vm.config.sockets`    | `1`                                    | Integer      | Number of CPU sockets allocated to the VM.                                 |
| `new_vm.config.tags`       | `['test']`                             | List         | Tags associated with the VM.                                               |
| `new_vm.format`            | `"qcow2"`                              | String       | Disk image format for the VM (e.g., `qcow2`, `raw`).                       |
| `new_vm.newid`             | `901`                                  | Integer      | Unique identifier for the new VM.                                          |
| `new_vm.storage`           | `"local-lvm"`                          | String       | Storage location for the VM.                                               |

Dependencies
------------

Ensure the required Python packages (python-setuptools, python-proxmoxer) are installed on the Ansible controller.

Example Playbook
-------

Here’s an example of how to use this role:

```yaml
- hosts: localhost
  remote_user: root
  roles:
    - role: pve_clone
      vars:
        pve_full_copy: false
        pve_host: "pve1.example.com"
        pve_node: "pve1"
        pve_pool: "dev"
        pve_port: 8006
        pve_state: "present"
        pve_templateid: 900
        pve_timeout: 300
        pve_tokenid: "root@pam!Ansible"

        # New VM configuration
        new_vm:
          - name: "clone-vm-1"
            config:
              ciupgrade: true
              ciuser: root
              cipassword: root
              cores: 4
              description: "Clone Virtual machine 1"
              disk_id: "virtio0"
              disk_size: "+5G"
              ipconfig:
                ipconfig0: 'ip=192.168.1.101/24,gw=192.168.1.1'
              kvm: false
              memory: 2048
              nameservers:
                - '192.168.1.1'
              net:
                net0: 'virtio,bridge=vmbr0'
              onboot: true
              searchdomains: 'example.com'
              sockets: 1
              tags:
                - 'test'
            format: qcow2
            newid: 901
            storage: "local-lvm"
```

License
-------

MIT

Author Information
------------------

- Christophe Vermeren | [GitHub](https://github.com/lorephoenix) | [Facebook](https://www.facebook.com/cvermeren)
