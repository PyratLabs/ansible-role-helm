# Ansible Role: helm

Ansible role for installing Helm.

[![Build Status](https://www.travis-ci.org/PyratLabs/ansible-role-helm.svg?branch=master)](https://www.travis-ci.org/PyratLabs/ansible-role-helm)

## Requirements

This role has been tested on Ansible 2.7.0+ against the following Linux Distributions:

  - Amazon Linux 2
  - CentOS 8
  - CentOS 7
  - Debian 10
  - Fedora 29
  - Fedora 30
  - Fedora 31
  - Ubuntu 18.04 LTS

## Disclaimer

If you have any problems please create a GitHub issue, I maintain this role in
my spare time so I cannot promise a speedy fix delivery.

:warning: This role only supports Helm v3.0.0+

## Role Variables


| Variable                       | Description                                                              | Default Value    |
|--------------------------------|--------------------------------------------------------------------------|------------------|
| `helm_version`                 | Use a specific version of helm, eg. `3.0.0`. Specify `false` for latest. | `false`          |
| `helm_install_os_dependencies` | Allow role to install OS dependencies.                                   | `false`          |
| `helm_install_dir`             | Installation directory for helm.                                         | `$HOME/bin`      |
| `helm_projects_dir`            | Directory to put helm charts from git. Specify `false` to skip.          | `$HOME/projects` |
| `helm_projects`                | List of helm charts to be cloned with `git`. See notes.                  | _NULL_           |

## Dependencies

No dependencies on other roles.

## Example Playbook

Example playbook for installing to single user:

```yaml
- hosts: control_hosts
  roles:
     - { role: xanmanning.helm, helm_version: 3.0.0 }
```

Example playbook for installing the latest helm version globally:

```yaml
---
- hosts: control_hosts
  become: true
  vars:
    helm_install_os_dependencies: true
    helm_install_dir: /opt/helm/bin
    helm_projects_dir: /opt/helm/projects
  roles:
    - role: xanmanning.helm
```

### Note about `helm_projects`

This is a list of git repositories to be cloned into the projects directory.
If this is empty, no projects will be cloned.

Below is an example of a project:

```yaml
helm_projects:
    - name: elastic-helm-charts                       # Directory name to clone into
      repo: git@github.com:elastic/helm-charts        # Repository to clone
      update_repo: true                               # Always update local copy of repo
      version:  master                                # Check out this version of the repo
      force: false                                    # Discard any existing working copy of the repo
      key_file: "{{ ansible_user_dir }}/.ssh/id_rsa"  # Key file to use to clone the repo
      recursive: true                                 # Include submodules in clone
```

## License

[BSD 3-clause](LICENSE.txt)

## Author Information

[Xan Manning](https://xanmanning.co.uk/)
