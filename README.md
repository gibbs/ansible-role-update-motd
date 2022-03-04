# Ansible Role: Update MOTD

![Build](https://github.com/gibbs/ansible-role-update-motd/actions/workflows/test.yml/badge.svg)
[![Ansible Role](https://img.shields.io/badge/Ansible%20Role-gibbs.update__motd-blue.svg)](https://galaxy.ansible.com/gibbs/update_motd)
[![License](https://img.shields.io/badge/License-MIT-brightgreen.svg)](https://opensource.org/licenses/MIT)

## Description

Manage dynamic MOTD scripts on Ubuntu and Debian.

## Installation

### Ansible Galaxy

Install the role from Ansible Galaxy:

```shell
$ ansible-galaxy install gibbs.update_motd
```

## Example

### Playbook

Playbook example:

```yaml
- hosts: all
  roles:
    - gibbs.update_motd
```

### Disable MOTD Scripts

Disable MOTD scripts by passing a list of filenames to 
`update_motd_disable_scripts`. All other scripts under `/etc/update-motd.d/` 
are enabled.

```yaml
- hosts: all
  roles:
    - gibbs.update_motd
  vars:
    update_motd_disable_scripts:
      - 88-esm-announce
      - 91-release-upgrade
      - 91-contract-ua-esm-status
```

### Adding Custom MOTD Scripts

This role will automatically enable any scripts under `/etc/update-motd.d/` that
are *not explicitly disabled*. How you add new scripts to hosts is entirely up 
to you:

```yaml
- hosts: all
  tasks:
    - name: copy my motd script
      ansible.builtin.copy:
        src: files/my-script.sh
        dest: /etc/update-motd.d/50-my-script
      notify: "update dynamic motd"
```

## Role Variables

All role variables that can be overriden are available in 
[defaults/main.yml](https://github.com/gibbs/ansible-role-update-motd/blob/master/defaults/main.yml)

| Name           | Default Value | Description                                 |
| -------------- | ------------- | --------------------------------------------|
| `update_motd_remove_motd_directory` | false | Whether to remove `/etc/motd` if it exists |
| `update_motd_package_name` | update-motd | The `update-motd` package name to manage (Ubuntu only) |
| `update_motd_package_state` | present | The `update-motd` package state (Ubuntu only) |
| `update_motd_service_state` | start | The `update-motd` service state (Ubuntu only) |
| `update_motd_service_enabled` | true | Whether the `update-motd` service should be enabled (Ubuntu only) |
| `update_motd_landscape_state` | present | The `landscape-common` package state (Ubuntu only) |
| `update_motd_disable_motd_service` | true | Whether to disable the motd service (if present) |
| `update_motd_disable_scripts` | 98-cloudguest | A list of MOTD script filenames to disable |

## Default MOTD scripts

A list of default MOTD script names commonly used in Debian and Ubuntu.

| Filename                    | Releases |
| --------------------------- | -------- |
| `00-header`                 | Ubuntu 14, 16, 18, 20 |
| `10-help-text`              | Ubuntu 14, 16, 18, 20 |
| `10-uname`                  | Debian 9, 10 |
| `50-landscape-sysinfo`      | Ubuntu 14, 18, 20 |
| `50-motd-news`              | Ubuntu 16, 18, 20 |
| `85-fwupd`                  | Ubuntu 20 |
| `88-esm-announce`           | Ubuntu 16, 18, 20 |
| `90-updates-available`      | Ubuntu 14, 16, 18 |
| `91-contract-ua-esm-status` | Ubuntu 16, 18, 20 |
| `91-release-upgrade`        | Ubuntu 14, 16, 18, 20 |
| `92-unattended-upgrades`    | Ubuntu 16, 18, 20 |
| `95-hwe-eol`                | Ubuntu 14, 18, 20 |
| `97-overlayroot`            | Ubuntu 14, 16, 18, 20 |
| `98-cloudguest`             | Ubuntu 14 |
| `98-fsck-at-reboot`         | Ubuntu 14, 16, 18, 20 |
| `98-reboot-required`        | Ubuntu 14, 16, 18, 20 |

## Supported Systems

- Debian 9 stretch
- Debian 10 buster
- Debian 11 bullseye
- Ubuntu 14.04 Trusty Tahr
- Ubuntu 16.04 Xenial Xerus
- Ubuntu 18.04 Bionic Beaver
- Ubuntu 20.04 Focal Fossa

## License

Licensed under MIT License. See 
[LICENSE](https://github.com/gibbs/ansible-role-update-motd/blob/master/LICENSE).
