# install_satellite

Ansible collection for installing and tuning satellite. Updated for Red Hat Satellute 6.8.

This collection is also available on [ansible galaxy](https://galaxy.ansible.com/jjaswanson4/install_satellite).

## Overview
This collection takes a RHEL7 host and installs satellite. It follows these steps:
- Configure repositories
- Remove RPMs that conflict with satellite RPMs
- Installs satellite RPMs
- Run the satellite installer
- Applies the specified tuning profile

## Usage
The best way to consume this collection is to set up a requirements.yml:
```yaml
---
collections:
  - jjaswanson4.install_satellite
```
The collection can also be installed from the command line ad-hoc:
```
ansible-galaxy collection install jjaswanson4.install_satellite
```

## Examples
- There are example playbooks located in the playbooks directory
- Vars examples can be found in playbooks/vars

## Vars
All vars are defined under `satellite`. There are a few required vars:
| Name             | Description                                                                                                   |
|------------------|---------------------------------------------------------------------------------------------------------------|
| `version`        | The version of satellite to install. Any supported version is supported, as well as `beta` for beta releases  |
| `admin_username` | The admin username for satellite                                                                              |
| `admin_password` | The admin password for satellite                                                                              |

In addition, the initial organization and location need to be defined under `satellite.foreman`. See below for the full structure.
Finally, be sure to include the `tuning_config_files` list, shown below.

```yaml
satellite:
  version: 6.8
  admin_username: admin
  admin_password: changeme
  tuning_profile: medium
  foreman:
    organizations:
      - name: general
        initial_organization: true # Required to be defined and set to true
      - name: josh
    locations:
      - name: josh1
        organizations:
          - name: josh
      - name: josh2
        organizations:
          - name: josh
          - name: general
      - name: msp-lab
        initial_location: true # Required to be defined and set to true
        organizations:
          - name: general
      - name: msp-lab2
        organizations:
          - name: josh
          - name: general
```

## Inventory Structure
This collection targets satellite servers, however it's a good idea to break up satellite/capsule servers and define var files for each individually:
```yaml
[satellite]
satellite67.josh.lab.msp.redhat.com vars_file=/home/jswanson/ansible/satellite6.7-collections/satellite67.josh.lab.msp.redhat.com.vars.yml ansible_user=root

[capsule]
capsule67-01.josh.lab.msp.redhat.com vars_file=/home/jswanson/ansible/satellite6.7-collections/capsule67-01.josh.lab.msp.redhat.com.vars.yml ansible_user=root
capsule67-02.josh.lab.msp.redhat.com vars_file=/home/jswanson/ansible/satellite6.7-collections/capsule67-02.josh.lab.msp.redhat.com.vars.yml ansible_user=root
```
