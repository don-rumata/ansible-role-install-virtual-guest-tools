# Ansible role: Install virtual guest tools 4 many VMs environments

[![License][license-image]][license-url] [![Ansible Galaxy][ansible-galaxy-image]][ansible-galaxy-url] [![Ansible Galaxy Quality][ansible-galaxy-quality-image]][ansible-galaxy-url] [![Ansible Galaxy Release][ansible-galaxy-release-image]][ansible-galaxy-url]

Install guest tools for VitualBox, QEMU\KVM, Xen, VMware, Parallels in Linux and Windows.

## Work on

### Ansible Galaxy style

```yaml
  platforms:
    - name: Fedora
      versions:
        - 33
        # TODO.
        # - 34
    - name: Ubuntu
      versions:
        - bionic
        - focal
    - name: Debian
      version:
        - buster
        - bullseye
        - stable
        - oldstable
    - name: EL
      versions:
        - 8
    - name: opensuse
      vesrion:
        - 15.3
        - tumbleweed
    - name: Windows
      version:
        - 2008x64 (7 64bit)
        - 2019 (10 64bit)
```

### Table style

- :heavy_check_mark: - work, tested, ok.
- :construction: - TODO. Work in progress.
- :x: - not work. Don't try.

|.              |VirtualBox        |QEMU\KVM          |Hyper-V           |VMware        |Xen               |Parallels     |
|---------------|------------------|------------------|------------------|--------------|------------------|--------------|
|**Ubuntu**     |                  |                  |                  |              |                  |              |
|focal          |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:construction:|:heavy_check_mark:|:construction:|
|bionic         |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:construction:|:construction:    |:construction:|
|**Debian**     |                  |                  |                  |              |                  |              |
|buster         |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:construction:|:heavy_check_mark:|:construction:|
|bullseye       |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:construction:|:heavy_check_mark:|:construction:|
|**EL (CentOS)**|                  |                  |                  |              |                  |              |
|8              |:heavy_check_mark:|:heavy_check_mark:|:construction:    |:construction:|:heavy_check_mark:|:construction:|
|**openSUSE**   |                  |                  |                  |              |                  |              |
|tumbleweed     |:heavy_check_mark:|:construction:    |:heavy_check_mark:|:construction:|:heavy_check_mark:|:construction:|
|leap           |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:construction:|:heavy_check_mark:|:construction:|
|**Fedora**     |                  |                  |                  |              |                  |              |
|33             |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:construction:|:construction:    |:construction:|
|34             |:construction:    |:construction:    |:construction:    |:construction:|:construction:    |:construction:|
|**Windows**    |                  |                  |                  |              |                  |              |
|7              |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:construction:|:construction:    |:construction:|
|10             |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:construction:|:heavy_check_mark:|:construction:|
|2012r2         |:construction:    |:heavy_check_mark:|:heavy_check_mark:|:construction:|:construction:    |:construction:|
|2019           |:construction:    |:heavy_check_mark:|:heavy_check_mark:|:construction:|:heavy_check_mark:|:construction:|

## Requirements

[min_ansible_version: 2.8](https://docs.ansible.com/ansible/latest/modules/flatpak_module.html)

## Role Variables

```yaml
---
#--- QEMU\KVM settings ---#

# Windows only
virtual_guest_tools_virtio_win_amd64_msi_url: https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/latest-virtio/virtio-win-gt-x64.msi
virtual_guest_tools_qemu_ga_win_amd64_msi_url: https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/latest-qemu-ga/qemu-ga-x86_64.msi
virtual_guest_tools_spice_webdavd_win_amd64_msi_url: https://www.spice-space.org/download/windows/spice-webdavd/spice-webdavd-x64-latest.msi

# Win7 only. https://askubuntu.com/a/1355273/457538
virtual_guest_tools_virtio_win_win7_amd64_msi_url: https://www.spice-space.org/download/windows/spice-guest-tools/spice-guest-tools-latest.exe

virtual_guest_tools_virtio_certs_urls:
  # - https://www.spice-space.org/download/windows/qxl-wddm-dod/qxl-wddm-dod-0.20/amd64/qxldod.cat
  - https://fedorapeople.org/groups/virt/unattended/drivers/preinst/virtio-win/0.1.171/w10/amd64/qxldod.cat
  - https://fedorapeople.org/groups/virt/unattended/drivers/preinst/virtio-win/0.1.171/w10/amd64/viostor.cat

#--- VirtualBox settings ---#

# https://github.com/don-rumata/ansible-role-install-virtualbox

virtualbox_edition: latest-stable
# virtualbox_edition: latest-beta
# virtualbox_edition: latest

virtualbox_url_prefix: https://download.virtualbox.org/virtualbox

virtualbox_url_version: '{{ virtualbox_url_prefix }}/{{ virtualbox_edition | upper }}.TXT'

virtualbox_url_path_to_files: '{{ virtualbox_url_prefix }}/{{ virtualbox_available_version_fact }}'

#--- Repo settings ---#

# If you *NOT* use apt-cacher-ng or other caching proxy - select "https".
http_or_https: http
# http_or_https: https
```

## Dependencies

### If you want deploy to Windows 7

Download and install [Windows Management Framework 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616)

## HowTo

Quick config WinRM for Windows: <https://ru.stackoverflow.com/a/949971/191416>

### How to install role

Over `ansible-galaxy`:

```bash
ansible-galaxy install don_rumata.ansible_role_install_virtual_guest_tools
```

Over `bash+git`:

```bash
mkdir -p "$HOME/.ansible/roles"
cd "$HOME/.ansible/roles"
git clone https://github.com/don-rumata/ansible-role-install-virtual_guest_tools don_rumata.ansible_role_install_virtual_guest_tools
```

## Example Playbooks

### I

Install guest tools for any [supported](#Work-on) plaforms:

`install-virtual-guest-tools.yml`:

```yaml
- name: Install virtual guest tools
  hosts: all
  strategy: free
  serial:
    - "100%"
  roles:
    - ansible-role-install-virtual-guest-tools
  tasks:
```

### II

Install guest tools from **local** web\smb server:

`install-virtual-guest-tools.yml`:

```yaml
  - name: Install Virtual Guest Agents
    hosts: all
    strategy: free
    serial:
      - "100%"
    roles:
      - role: ansible-role-install-virtual-guest-tools
        virtual_guest_tools_virtio_win_amd64_msi_url: \\10.10.10.10\soft\virtio-win\latest-virtio\virtio-win-gt-x64.msi
        virtual_guest_tools_qemu_ga_win_amd64_msi_url: \\10.10.10.10\soft\virtio-win\latest-qemu-ga\qemu-ga-x86_64.msi
        virtual_guest_tools_virtio_win_win7_amd64_msi_url: http://10.10.10.10/soft/spice/spice-guest-tools-latest.exe
        virtual_guest_tools_spice_webdavd_win_amd64_msi_url: http://10.10.10.10/soft/spice/spice-webdavd-x64-latest.msi
        virtual_guest_tools_virtio_certs_urls:
          - http://10.10.10.10/drivers/other/virtio-win/qxldod/w10/amd64/qxldod.cat
          - http://10.10.10.10/drivers/other/virtio-win/viostor/w10/amd64/viostor.cat
    tasks:
```

## Development and testing environments

**VirtualBox**: VirtualBox v5.2.44

**KVM**: Proxmox v6.4-13

**Xen**: XCP-ng v8.2

**Hyper-V**: Windows server 2019 (ru_windows_server_2019_updated_april_2021_x64_dvd_479f8ca4.iso)

## Workarounds (dirty hacks)

Debian `xe-guest-utilities` will be installed from `ubuntu/pool/main`.

openSUSE `xe-guest-utilities` will be installed from latest `EPEL` repo.

## Known issue

### `hv-fcopy-daemon.service` not started.

On Hyper-V host:

```powershell
Enable-VMIntegrationService 'Guest Service Interface' -VMName $MY_VM_NAME
```

[src](https://bugs.launchpad.net/ubuntu/+source/linux/+bug/1614618/comments/6)

## License

Apache License, Version 2.0

## Author Information

[don Rumata](https://github.com/don-rumata)

## TODO

- Add tests.

[license-image]: https://img.shields.io/github/license/don-rumata/ansible-role-install-virtual-guest-tools.svg
[license-url]: https://opensource.org/licenses/Apache-2.0

[ansible-galaxy-image]: https://img.shields.io/badge/ansible_galaxy-don__rumata.ansible__role__install__virtual-guest-tools-blue.svg
[ansible-galaxy-url]: https://galaxy.ansible.com/don_rumata/ansible_role_install_virtual-guest-tools

[ansible-galaxy-quality-image]: https://img.shields.io/ansible/quality/56147

[ansible-galaxy-release-image]: https://img.shields.io/github/v/release/don-rumata/ansible-role-install-virtual-guest-tools.svg?include_prereleases
