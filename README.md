<p><img src="https://cdn.worldvectorlogo.com/logos/minio-1.svg" alt="minio logo" title="minio" align="right" height="60" /></p>

# Ansible Role: Minio

[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Ansible Role](https://img.shields.io/badge/ansible%20role-joedissmeyer.ansible_role_minio-blue.svg)](https://galaxy.ansible.com/joedissmeyer/ansible_role_minio)

## Description

Deploy a bare-bones, single-node [Minio](https://min.io/) object storage instance using ansible. The purpose of this role is to quickly and efficiently install Minio on a single virtual machine. Minimal variables are used to achieve this goal.

## Requirements

- Ansible >= 2.12 (It might work on previous versions.)
- Install the role, `ansible-galaxy install joedissmeyer.ansible_role_minio`

## Role Variables

All variables which can be overridden are stored in [defaults/main.yml](defaults/main.yml) file as well as in table below.

Several variables reference the official configuration items needed by Minio server to function. More details about every environment variable for Minio server can be [found here](https://docs.min.io/minio/baremetal/reference/minio-server/minio-server.html).

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `minio_home` | /opt/minio | Home directory for the minio binaries and configuration files. |
| `minio_server_download_source` | "https://dl.min.io/server/minio/release/linux-amd64/minio" | Download source of the Minio server binary. |
| `minio_client_download_source` | "https://dl.min.io/client/mc/release/linux-amd64/mc" | Download source of the Minio CLI client. |
| `minio_data_path` | '{{ minio_home }}/data' | Path to directory where Minio will store buckets and object data. Translates to the `MINIO_VOLUMES` configuration variable in the Minio config file. Currently is _static_ to a single item. |
| `minio_server_port`| "9000" | The TCP port that Minio server will bind to for the Object storage (S3) API. |
| `minio_console_port` | "9001" | The TCP port that the Minio server will bind the web console ui. |
| `minio_root_user` | minioadmin | The internal root user account user name. See [MINIO_ROOT_USER](https://docs.min.io/minio/baremetal/reference/minio-server/minio-server.html#envvar.MINIO_ROOT_USER) in Min.io docs. |
| `minio_root_password` | minioadmin | The internal root user password. See [MINIO_ROOT_PASSWORD](https://docs.min.io/minio/baremetal/reference/minio-server/minio-server.html#envvar.MINIO_ROOT_PASSWORD) in Min.io docs. |
| `minio_user` | minio-user | This is the service account used by the Minio systemd unit. |
| `minio_group` | minio-user | This is the group name used by the Minio systemd unit. |
| `minio_ssl_enabled` | false | Intended to be used for enabling TLS in a future version of this role. Not used at this time. |
| `minio_prometheus_auth_type` | public | Specifies the authentication mode for the Prometheus scraping endpoints. Only two options are `jwt` or `public`. See [MINIO_PROMETHEUS_AUTH_TYPE](https://docs.min.io/minio/baremetal/reference/minio-server/minio-server.html#envvar.MINIO_PROMETHEUS_AUTH_TYPE) in Min.io docs for more info |

## Example

Simply include the role in your playbook.

First, make sure the role is installed.
You can install via Ansible Galaxy or via requirements file.
To install using Galaxy,

```shell
$ ansible-galaxy install joedissmeyer.ansible_role_minio
```

Or using a requirements file:

```
roles:
- name: joedissmeyer.ansible_role_minio
  src: https://github.com/joedissmeyer/ansible-role-minio.git
```

You may include any variable overrides in `vars` as needed.
It would be wise to change the minio root user account and password using Ansible Vault at the minimum.

```
- name: Install Minio
  become: yes
  gather_facts: yes
  hosts: minio
  roles:
    - joedissmeyer.ansible_role_minio
  vars:
    minio_root_user: minioadmin
    minio_root_password: minioadmin
    minio_server_download_source: "https://dl.min.io/server/minio/release/linux-amd64/minio"
    minio_client_download_source: "https://dl.min.io/client/mc/release/linux-amd64/mc"
```

## License

This project is licensed under the Apache 2.0 license. See [LICENSE](/LICENSE) for more details.