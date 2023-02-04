# Ansible role: Install virtual guest tools 4 many VMs environments

[![License][license-image]][license-url] [![Ansible Galaxy][ansible-galaxy-image]][ansible-galaxy-url] [![Ansible Galaxy Quality][ansible-galaxy-quality-image]][ansible-galaxy-url] [![Ansible Galaxy Release][ansible-galaxy-release-image]][ansible-galaxy-url]

Install guest tools for VirtualBox, QEMU\KVM, Xen, VMware in Linux and Windows.

## Work on

### Ansible Galaxy style

```yaml
  platforms:
    - name: Fedora
      versions:
        - 33
        - 34
        - 35
    - name: Ubuntu
      versions:
        - jammy
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
- :thinking: - not tested, but it should work.

|.              |VirtualBox        |QEMU\KVM          |Hyper-V           |VMware            |Xen               |
|---------------|------------------|------------------|------------------|------------------|------------------|
|**Ubuntu**     |                  |                  |                  |                  |                  |
|jammy          |:heavy_check_mark:|:heavy_check_mark:|:thinking:        |:thinking:        |:thinking:        |
|focal          |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|
|bionic         |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:thinking:        |:construction:    |
|**Debian**     |                  |                  |                  |                  |                  |
|buster         |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:thinking:        |:heavy_check_mark:|
|bullseye       |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|
|**EL (CentOS)**|                  |                  |                  |                  |                  |
|8              |:heavy_check_mark:|:heavy_check_mark:|:construction:    |:thinking:        |:heavy_check_mark:|
|**openSUSE**   |                  |                  |                  |                  |                  |
|tumbleweed     |:heavy_check_mark:|:construction:    |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|
|leap           |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|
|**Fedora**     |                  |                  |                  |                  |                  |
|33             |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:thinking:        |:construction:    |
|34             |:thinking:        |:thinking:        |:thinking:        |:thinking:        |:thinking:        |
|35             |:thinking:        |:thinking:        |:thinking:        |:heavy_check_mark:|:thinking:        |
|**Windows**    |                  |                  |                  |                  |                  |
|7              |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:construction:    |
|10             |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|
|2012r2         |:construction:    |:heavy_check_mark:|:heavy_check_mark:|:thinking:        |:construction:    |
|2019           |:construction:    |:heavy_check_mark:|:heavy_check_mark:|:thinking:        |:heavy_check_mark:|

## Requirements

min_ansible_version: 2.8

## Role Variables

```yaml
---
#--- QEMU\KVM settings ---#

# Windows only.
virtual_guest_tools_virtio_win_amd64_msi_url: https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/latest-virtio/virtio-win-gt-x64.msi
virtual_guest_tools_qemu_ga_win_amd64_msi_url: https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/latest-qemu-ga/qemu-ga-x86_64.msi
virtual_guest_tools_spice_webdavd_win_amd64_msi_url: https://www.spice-space.org/download/windows/spice-webdavd/spice-webdavd-x64-latest.msi

# win7 only. https://askubuntu.com/a/1355273/457538
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

virtualbox_url_path_to_iso: https://download.virtualbox.org/virtualbox/{{ virtualbox_available_version_fact }}/VBoxGuestAdditions_{{ virtualbox_available_version_fact }}.iso
# virtualbox_url_path_to_iso: http://10.10.10.10/soft/virtualbox/latest-stable/vboxguestadditions_latest.iso

#--- Xen settings ---#

# Windows only.
# https://xenappblog.com/2018/download-and-install-latest-citrix-xenserver-tools/
virtual_guest_tools_xen_win_tsv_url: https://pvupdates.vmd.citrix.com/updates.latest.tsv
virtual_guest_tools_xen_win_tsv_path_to_save: /tmp/xen.updates.latest.tsv

# Value for custom location
# virtual_guest_tools_xen_win_management_agent_url: http://10.10.10.10/soft/citrix/management-agent/managementagentx64.msi
# virtual_guest_tools_xen_win_management_agent_url: \\10.10.10.10\soft\citrix\management-agent\managementagentx64.msi

#--- Hyper-V settings ---#

# Windows only.
# win7
virtual_guest_tools_hyper_v_win7_cab_url: https://download.microsoft.com/download/0/C/A/0CADD11C-CE3A-4BF6-8321-165438638676/windows6.x-hypervintegrationservices-x64.cab
virtual_guest_tools_hyper_v_win2012_cab_url: https://download.microsoft.com/download/5/A/C/5AC3E8C3-CB7C-413E-A518-8D0C56042102/windows6.2-hypervintegrationservices-x64.cab

#--- VMware settings ---#

virtual_guest_tools_vmware_versions_url: https://packages.vmware.com/tools/versions

# If the value is not defined, the last supported version from url will be selected.
# vmware_tools_version:

# If the value is not defined, the last supported build from url will be selected.
# vmware_tools_build_number:

virtual_guest_tools_vmware_win_url: https://packages.vmware.com/tools/releases/{{ vmware_tools_version_fact }}/windows/x64/VMware-tools-{{ vmware_tools_version_fact }}-{{ vmware_tools_build_number_fact }}-x86_64.exe
# virtual_guest_tools_vmware_win_url: http://10.10.10.10/soft/vmware/vmware-tools-latest-amd64.exe
# virtual_guest_tools_vmware_win_url: \\10.10.10.10\soft\vmware\vmware-tools-latest-amd64.exe

virtual_guest_tools_vmware_win_reboot_after_install: true
# virtual_guest_tools_vmware_win_reboot_after_install: false

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
    - don_rumata.ansible_role_install_virtual_guest_tools
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
      - role: don_rumata.ansible_role_install_virtual_guest_tools
        virtual_guest_tools_virtio_win_amd64_msi_url: \\10.10.10.10\soft\virtio-win\latest-virtio\virtio-win-gt-x64.msi
        virtual_guest_tools_qemu_ga_win_amd64_msi_url: \\10.10.10.10\soft\virtio-win\latest-qemu-ga\qemu-ga-x86_64.msi
        virtual_guest_tools_virtio_win_win7_amd64_msi_url: http://10.10.10.10/soft/spice/spice-guest-tools-latest.exe
        virtual_guest_tools_spice_webdavd_win_amd64_msi_url: http://10.10.10.10/soft/spice/spice-webdavd-x64-latest.msi
        virtual_guest_tools_virtio_certs_urls:
          - http://10.10.10.10/drivers/other/virtio-win/qxldod/w10/amd64/qxldod.cat
          - http://10.10.10.10/drivers/other/virtio-win/viostor/w10/amd64/viostor.cat
        virtual_guest_tools_xen_win_management_agent_url: \\10.10.10.10\soft\citrix\management-agent\managementagentx64.msi

        virtualbox_url_path_to_iso: http://10.10.10.10/soft/virtualbox/latest-stable/vboxguestadditions_latest.iso

        virtual_guest_tools_vmware_versions_url: http://10.10.10.10/soft/vmware/tools/versions
        virtual_guest_tools_vmware_win_url: http://10.10.10.10/soft/vmware/tools/vmware-tools-latest-amd64.exe
    tasks:
```

`virtual-server-inventory.ini`:

```ini
[my-vbox-sandboxs]
10.10.1.10
10.10.1.20
10.10.1.30

[my-kvm-vms]
10.10.2.10
10.10.2.20
10.10.3.30

[my-xen-tests]
10.10.3.10
10.10.3.20
10.10.3.30

[my-hyper-v-dumpster]
10.10.4.10
10.10.4.20
10.10.4.30

[my-vmware-zoo]
10.10.5.10
10.10.5.20
10.10.5.30
```

```bash
ansible-playbook -i ./virtual-server-inventory.ini ./install-virtual-guest-tools.yml
```

## Development and testing environments

**VirtualBox**: VirtualBox v5.2.44

**KVM**: Proxmox v6.4-13

**Xen**: XCP-ng v8.2

**Hyper-V**: Windows server 2019 (ru_windows_server_2019_updated_april_2021_x64_dvd_479f8ca4.iso)

**VMware**: VMware-player-full-16.2.2-19200509.exe

## Workarounds (dirty hacks)

Debian `xe-guest-utilities` will be installed from `ubuntu/pool/main`.

openSUSE `xe-guest-utilities` will be installed from latest `EPEL` repo.

## Known issue

### `hv-fcopy-daemon.service` not started

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
- ~~Add var 4 vbox iso~~

[license-image]: https://img.shields.io/github/license/don-rumata/ansible-role-install-virtual-guest-tools.svg
[license-url]: https://opensource.org/licenses/Apache-2.0

[ansible-galaxy-image]: https://img.shields.io/badge/ansible_galaxy-don__rumata.ansible__role__install__virtual__guest__tools-blue.svg
[ansible-galaxy-url]: https://galaxy.ansible.com/don_rumata/ansible_role_install_virtual_guest_tools

[ansible-galaxy-quality-image]: https://img.shields.io/ansible/quality/56147

[ansible-galaxy-release-image]: https://img.shields.io/github/v/release/don-rumata/ansible-role-install-virtual-guest-tools.svg?include_prereleases
