IBM Spectrum Protect Client
=========

This role deploys IBM Spectrum Protect client to be used by
Storware Kodo for Endpoints.

Requirements
------------

CentOS Stream/RHEL 8 minimal installation and public key authentication between command host and target machine

Role Variables
--------------

Defaults:
```
isp_client_version: "8.1.12.0"
isp_short_version: "{{ isp_client_version | regex_replace('([0-9]+)[.]([0-9]+)[.]([0-9]+).*', 'v\\1r\\2') }}"
isp_client_short_version: "{{ isp_client_version | regex_replace('([0-9]+)[.]([0-9]+)[.]([0-9]+).*', 'v\\1\\2\\3') }}"
isp_base_download_url: "http://ftp.software.ibm.com/storage/tivoli-storage-management/maintenance"
isp_client_download_url: "{{ isp_base_download_url }}/client/{{ isp_short_version }}/Linux/LinuxX86/BA/{{ isp_client_short_version }}/{{ isp_client_installer_filename }}"
isp_client_installer_local_dir: "/tmp/isp-client"
isp_client_installer_local_path: "{{ isp_client_installer_local_dir }}/{{ isp_client_installer_filename }}"
isp_dir: "/opt/tivoli/tsm"
isp_dsmopt_path: "{{ isp_dir }}/client/ba/bin/dsm.opt"
isp_dsmsys_path: "{{ isp_dir }}/client/ba/bin/dsm.sys"
isp_servername: "isp"
isp_tcpserveraddress: "localhost"
isp_port: 1500
isp_admin_port: 1502
```

Key variables:

* `isp_client_download_url` - URL to be used to download ISP client (download may be slow,
  so you can also download it manually, upload it to the remote machine and set this variable
  to something like `file:///tmp/8.1.12.0-TIV-TSMBAC-LinuxX86.tar` which will copy installer from local
  directory instead of downloading it from the Internet)
* `isp_dir`- path where ISP client is going to be installed
* `isp_dsmopt_path` - path to the `dsm.opt` file
* `isp_dsmsys_path` - path to the `dsm.sys` file
* `isp_servername` - name of the server in `dsm.opt` and `dsm.sys` files
* `isp_tcpserveraddress` - address of the ISP server
* `isp_tcpport` - port of the ISP server
* `isp_tcpadminport`- administrative port of the ISP server

Dependencies
------------

N/A

Example Playbook
----------------

This deploys IBM Spectrum Protect client on `isp` host (only one server can be deployed)
and multiple agents on `agents` hosts

```
- hosts: isp
  roles:
   - xe0nic.ansible_isp_client
```

Example hosts inventory (you need to make sure that SSH public key authentication for
ansible user provided in inventory is configured):

```
[all:vars]
ansible_user = root

[isp]
192.168.155.233
```

License
-------

MIT

Author Information
------------------

For more information visit product website: https://storware.eu/products/kodo-for-endpoints
Documentation: https://storware.gitbook.io/kodo-for-endpoints