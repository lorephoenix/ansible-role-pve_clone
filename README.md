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

| Variable | Default Value | Data Type | Description |
| :--- | :--- | :--- | :---|
| `pve_full_copy`  | `true`               | Boolean               | Whether to create a full copy of the VM.         |
| `pve_host`       | `proxmox.example.com`| String                | Proxmox server hostname or IP address.           |
| `pve_node`       | `pve`                | String                | The target Proxmox node for VM creation.         |
| `pve_port`       | `8006`               | Integer               |	Proxmox API port.                             |
| `pve_state`      | `present`            | String                | Desired state of the VM (`present` or `absent`). |
| `pve_templateid` | `100`                | Integer               | The ID of the Proxmox VM template to clone.      |
| `pve_timeout`    | `500`                | Integer               | Timeout for API requests in seconds.             |
| `pve_tokenid`    | `root@pam!mytokenid` | String                | API token ID for authentication.                 |
| `new_vm`         | See example below    | List of dictionaries  | List of VM configurations for cloning.           |

### `new_vm` Example:

```yaml
new_vm:
  - name: "clone-vm-01"
    format: raw
    storage: "local-lvm"
```

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
