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

<table>
  <thead style="background-color: #3e3e3e; color: white;">
    <tr>
      <th>Variable</th>
      <th>Default Value</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>`pve_host`</td>
      <td>`proxmox.example.com`</td>
      <td>The hostname or IP address of the Proxmox server.</td>
    </tr>
    <tr>
      <td>`pve_port`</td>
      <td>`8006`</td>
      <td>The port used for Proxmox API communication.</td>
    </tr>
    <tr>
      <td>`pve_node`</td>
      <td>`pve`</td>
      <td>The target Proxmox node for VM creation.</td>
    </tr>
    <tr>
      <td>`pve_tokenid`</td>
      <td>`root@pam!mytokenid`</td>
      <td>API token ID for authentication.</td>
    </tr>
    <tr>
      <td>`pve_templateid`</td>
      <td>`100`</td>
      <td>The ID of the Proxmox VM template to clone.</td>
    </tr>
    <tr>
      <td>`pve_full_copy`</td>
      <td>`true`</td>
      <td>Whether to create a full copy of the VM.</td>
    </tr>
    <tr>
      <td>`pve_timeout`</td>
      <td>`500`</td>
      <td>Timeout for API requests in seconds.</td>
    </tr>
    <tr>
      <td>`pve_state`</td>
      <td>`present`</td>
      <td>Desired state of the VM (`present` or `absent`).</td>
    </tr>
    <tr>
      <td>`new_vm`</td>
      <td>See example below</td>
      <td>List of VM configurations for cloning.</td>
    </tr>
  </tbody>
</table>



| Variable | Default Value | Description |
| :--- | :--- | :---|
| `pve_host`       | `proxmox.example.com` | Proxmox server hostname or IP address.           |
| `pve_port`       | `8006`                |	Proxmox API port.                             |
| `pve_node`       | `pve`                 | The target Proxmox node for VM creation.         |
| `pve_tokenid`    | `root@pam!mytokenid`  | API token ID for authentication.                 |
| `pve_templateid` | `100`                 | The ID of the Proxmox VM template to clone.      |
| `pve_full_copy`  | `true`                | Whether to create a full copy of the VM.         |
| `pve_timeout`    | `500`                 | Timeout for API requests in seconds.             |
| `pve_state`      | `present`             | Desired state of the VM (`present` or `absent`). |
| `new_vm`         | See example below     | List of VM configurations for cloning.           |


Dependencies
------------

Ensure the required Python packages (python-setuptools, python-proxmoxer) are installed on the Ansible controller.

License
-------

MIT

Author Information
------------------

- Christophe Vermeren | [GitHub](https://github.com/lorephoenix) | [Facebook](https://www.facebook.com/cvermeren)
