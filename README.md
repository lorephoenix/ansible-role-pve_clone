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

| Variable | Data Types | Default Value | Description |
| :--- | :--- | :--- | :---|
| `pve_host`       | String     | `proxmox.example.com` | Proxmox server hostname or IP address.           |
| `pve_port`       | Integer    | `8006`                |	Proxmox API port.                             |
| `pve_node`       | String     | `pve`                 | The target Proxmox node for VM creation.         |
| `pve_tokenid`    | String     | `root@pam!mytokenid`  | API token ID for authentication.                 |
| `pve_templateid` | Integer    | `100`                 | The ID of the Proxmox VM template to clone.      |
| `pve_full_copy`  | Boolean    | `true`                | Whether to create a full copy of the VM.         |
| `pve_timeout`    | Integer    | `500`                 | Timeout for API requests in seconds.             |
| `pve_state`      | String     | `present`             | Desired state of the VM (`present` or `absent`). |
| `new_vm`         | List       | See example below     | List of VM configurations for cloning.           |


Dependencies
------------

Ensure the required Python packages (python-setuptools, python-proxmoxer) are installed on the Ansible controller.

License
-------

MIT

Author Information
------------------

- Christophe Vermeren | [GitHub](https://github.com/lorephoenix) | [Facebook](https://www.facebook.com/cvermeren)
