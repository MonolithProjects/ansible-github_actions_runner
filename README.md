# GitHub Actions Runner

[![Galaxy Quality](https://img.shields.io/ansible/quality/47375?style=flat&logo=ansible)](https://galaxy.ansible.com/monolithprojects/github_actions_runner)
[![Role version](https://img.shields.io/github/v/release/MonolithProjects/ansible-github_actions_runner)](https://galaxy.ansible.com/monolithprojects/github_actions_runner)
[![Role downloads](https://img.shields.io/ansible/role/d/47375)](https://galaxy.ansible.com/monolithprojects/github_actions_runner)
[![GitHub Actions](https://github.com/MonolithProjects/ansible-github_actions_runner/workflows/molecule%20test/badge.svg?branch=master)](https://github.com/MonolithProjects/ansible-github_actions_runner/actions)
[![License](https://img.shields.io/github/license/MonolithProjects/ansible-github_actions_runner)](https://github.com/MonolithProjects/ansible-github_actions_runner/blob/master/LICENSE)

This role will deploy or redeploy or uninstall and register or unregister local GitHub Actions Runner.

## Requirements

* Supported Linux distros:
  * CentOS/RHEL 7,8
  * Debian 9,10
  * Fedora 31,30,29,28
  * Ubuntu 16,18

* System must have access to the GitHub.

* The role require Personal Access Token for the GitHub user. The token has to be a value of `PERSONAL_ACCESS_TOKEN` variable.
Export the token to the local host environment. The token has to have admin rights for the repo.  
Personal Access Token for GitHub account can be created [here](https://github.com/settings/tokens).  
**Note:** Never store you personal access token in the GitHub repository. Use [GitHub Secrets](https://help.github.com/en/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets) or some different secrets service.

* Runner user has to be pre-created.  
  Recommended role: `monolithprojects.user_management`

* CentOS/Fedora systems require EPEL repository.  
  Recommended role: `robertdebock.epel`

## Role Variables

This is a copy from `defaults/main.yml`

```yaml
# Runner user - user under which is the local runner service running
runner_user: "{{ lookup('env','USER') }}"

# Directory where the local runner will be installed
runner_dir: /opt/actions-runner

# Version of the GitHub Actions Runner
runner_version: "latest"

# If found, replace already registered runner
replace_runner: yes

# Do not show Ansible logs which may contain sensitive data (registration token)
hide_sensitive_logs: yes

# Personal Access Token for your GitHub account
access_token: "{{ lookup('env', 'PERSONAL_ACCESS_TOKEN') }}"

# GitHub address
github_server: "https://github.com"

# GitHub account name
# github_account: "youruser"

# Github repository name
# github_repo: "yourrepo"
```

## Example Playbook 

In this example the role will deploy (or redeploy) the GitHub Actions runner service (latest available version) and register the runner for the GitHub repo.
Runner service will run under the same user as the Ansible is using for ssh connection (*ansible*).

```yaml
---
- name: GitHub Actions Runner
  hosts: all
  user: ansible
  become: yes
  vars:
    - github_account: my-github-user
    - github_repo: my_awesome_repo
  roles:
    - role: monolithprojects.github_actions_runner
```

In this example the role will deploy (or redeploy) the GitHub Actions runner service (version 2.165.2) and register the runner for the GitHub repo.

```yaml
---
- name: GitHub Actions Runner
  hosts: all
  become: yes
  vars:
    - runner_version: "2.165.2"
    - runner_user: runner-user
    - github_account: my-github-user
    - github_repo: my_awesome_repo
  roles:
    - role: monolithprojects.github_actions_runner
```

By using tag `uninstall`, GitHub Actions runner will be removed from the host and unregistered from the GitHub.

```bash
ansible-playbook playbook.yml --tags uninstall
```

License
-------

MIT

Author Information
------------------

Created in 2020 by Michal Muransky
