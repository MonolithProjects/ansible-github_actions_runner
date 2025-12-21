# GitHub Actions Runner

[![awesome-runners](https://img.shields.io/badge/listed%20on-awesome--runners-blue.svg)](https://github.com/jonico/awesome-runners)
[![Role version](https://img.shields.io/github/v/release/MonolithProjects/ansible-github_actions_runner)](https://galaxy.ansible.com/ui/standalone/roles/monolithprojects/github_actions_runner/)
[![Role downloads](https://img.shields.io/ansible/role/d/MonolithProjects/github_actions_runner)](https://galaxy.ansible.com/ui/standalone/roles/monolithprojects/github_actions_runner/)
[![Molecule test](https://github.com/MonolithProjects/ansible-github_actions_runner/actions/workflows/tests.yml/badge.svg)](https://github.com/MonolithProjects/ansible-github_actions_runner/actions/workflows/tests.yml)
[![License](https://img.shields.io/github/license/MonolithProjects/ansible-github_actions_runner)](https://github.com/MonolithProjects/ansible-github_actions_runner/blob/main/LICENSE)

This role will deploy/redeploy/uninstall and register/unregister local GitHub Actions Runner on Linux and macOS systems (see [compatibility list](#supported-operating-systems)).
It supports Enterprise, Organization and Repository Runners.

## Requirements

* System must have access to the GitHub API.

* The role requires a Personal Access Token to access GitHub. The token can be set as the `PERSONAL_ACCESS_TOKEN` environment variable.

> **Note**  
> The token must have the `repo` scope (when creating a repo runner), the `admin:org` scope (when creating a runner for an organization),
> or the `manage_runners:enterprise` scope (when creating an enterprise runner).
> Personal Access Token for GitHub account can be created [here](https://github.com/settings/tokens).

> **Warning**  
> Never store your personal access token in the GitHub repository. Use [GitHub Secrets](https://help.github.com/en/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets) or a different secrets service.

* Runner user has to be pre-created.
  Recommended role: `monolithprojects.user_management`

* CentOS and Rocky Linux systems require EPEL repository.
  Recommended role: `robertdebock.epel`

## Supported CPU Architecture

* ARM, ARM64 (dependencies installation is not covered by this role)
* AMD64, x86_64

## Supported Operating Systems

* Red Hat Enterprise Linux 8+
* Rocky Linux 8+
* Fedora 40+
* Debian 10+
* Ubuntu 22.04+
* macOS High Sierra+
* Windows

## Tested On

* Debian 13
* Fedora 43
* Rocky Linux 9
* Ubuntu 24

## Role Variables

This is a copy of `defaults/main.yml`

```yaml
---
# Runner user - user under which is the local runner service running
runner_user: "{{ lookup('env', 'USER') }}"

# Directory where the local runner will be installed
runner_dir: "{{ 'C:\\actions-runner' if ansible_facts.system == 'Win32NT' else '/opt/actions-runner' }}"

# Version of the GitHub Actions Runner
runner_version: "latest"

# State in which the runner service will be after the role is done (started, stopped, absent)
runner_state: "started"

# If found on the server, delete already existing runner service and install it again
reinstall_runner: false

# Do not show Ansible logs which may contain sensitive data (registration token)
hide_sensitive_logs: true

# GitHub address
github_url: "https://github.com"

# GitHub API
github_api_url: "https://api.github.com"

# Number of runners to list per page
github_api_runners_per_page: 100

# Personal Access Token for your GitHub account
access_token: "{{ lookup('env', 'PERSONAL_ACCESS_TOKEN') }}"

# Is it the runner for organization or not?
runner_org: false

# Labels to apply to the runner
runner_labels: []

# Group to add organization runner to
runner_group: ""

# GitHub Actions Runner repository (change it if you want to use custom Actions Runner fork)
runner_download_repository: "actions/runner"

# Extra arguments to pass to `config.sh`.
# Several arguments must be set as one string (i.e. "--ephemeral --my_special_fork")
runner_extra_config_args: ""

# Disable default labels (self-hosted, Linux, X64) and require custom labels. Set `runner_no_default_labels: true` and provide at least one label in `runner_labels` to use this feature.
runner_no_default_labels: false

# Name to assign to this runner in GitHub (System hostname as default)
runner_name: "{{ ansible_facts.hostname }}"

# Set to false when provisioning runners for more than one repository within single play
all_runners_in_same_repo: true

# GitHub Repository user or Organization owner used for Runner registration
# github_account: "youruser"

# GitHub repository owner name (if other than github_account)
# github_owner: "yourorg"

# Github repository name
# github_repo: "yourrepo"

# GitHub Enterprise name
# github_enterprise: "yourenterprise"

# Runner user Windows password - the logon password for the service user when running on windows.
# runner_user_win_password: "{{ lookup('env', 'PASS') }}"

# Configuring a custom .env file
# custom_env: |
# http_proxy=YOUR_URL_HERE
# ftp_proxy=YOUR_URL_HERE
# HTTPS_PROXY=YOUR_URL_HERE
# https_proxy=YOUR_URL_HERE
# no_proxy=localhost,127.0.0.1,127.0.0.2
# HTTP_PROXY=
```

## Example Playbooks

In this example the Ansible role will install (or update) the GitHub Actions Runner service (latest available version). The runner will be registered for the *my_awesome_repo* GitHub repository.
The runner service will be started and will run under the same user as Ansible is using for SSH connection (*ansible*).

```yaml
---
- name: Install GitHub Actions Runner
  hosts: all
  user: ansible
  become: yes
  vars:
    github_account: github-access-user
    github_repo: my_awesome_repo
  roles:
    - role: monolithprojects.github_actions_runner
```

Same example as above, but the runner will be added to an organization and deployed on GitHub Enterprise Server.

```yaml
---
- name: Install GitHub Actions Runner
  hosts: all
  user: ansible
  become: yes
  vars:
    github_account: my_awesome_org
    runner_org: yes
    runner_on_ghes: yes
  roles:
    - role: monolithprojects.github_actions_runner
```

If you have a GitHub Enterprise Cloud license and you want to manage all the self-hosted runners from the enterprise:

```yaml
---
- name: Install GitHub Actions Runner
  hosts: all
  user: automation
  become: yes
  vars:
    github_enterprise: my_awesome_enterprise
    runner_org: no
  roles:
    - role: monolithprojects.github_actions_runner
```

In this example the Ansible role will deploy (or update) the GitHub Actions runner service (version 2.165.2) and register the runner for the GitHub repository. The runner service will run under the user `runner-user`. The runner will be registered with two labels.
The runner service will be *stopped* and disabled. The runner will use custom environment variables (from a file named `.env` in the self-hosted runner application directory).

```yaml
---
- name: Stop GitHub Actions Runner
  hosts: all
  become: yes
  vars:
    runner_version: "2.165.2"
    runner_user: runner-user
    github_account: github-access-user
    github_repo: my_awesome_repo
    runner_state: "stopped"
    runner_labels:
      - production
      - west
    custom_env: |
      HTTP_PROXY=http://proxy.local:8080
      http_proxy=http://proxy.local:8080
      HTTPS_PROXY=http://proxy.local:8080
      https_proxy=http://proxy.local:8080
      no_proxy=localhost,127.0.0.1,127.0.0.2

  roles:
    - role: monolithprojects.github_actions_runner
```

In this example the Ansible role will uninstall the runner service and unregister it from the GitHub repository.

```yaml
---
- name: Uninstall GitHub Actions Runner
  hosts: all
  become: yes
  vars:
    github_account: github-access-user
    github_repo: my_awesome_repo
    runner_state: "absent"
  roles:
    - role: monolithprojects.github_actions_runner
```

## Testing with Molecule

[Molecule](https://molecule.readthedocs.io/) is a testing framework for Ansible. This section is for code contributors.

### Prerequisites

* Python
* Docker
* Ansible
* Molecule

### Installation

1. Install Python, Docker, and Ansible if you haven't already.
2. Install Molecule and its Docker driver with pip:

```bash
pip install molecule[docker]
pip install "molecule-plugins[docker]"
```

### Running Tests

1. Navigate to the role's directory:

```bash
cd path/to/monolithprojects.github_actions_runner
```

2. Set environment variables:

```bash
export PERSONAL_ACCESS_TOKEN=your_github_pat # Your Personal Access Token to GitHub
export GITHUB_ACCOUNT=your_account # Your GitHub Account
export GITHUB_REPO=your_repository # GitHub Repository where you want to set up the Runner
```

3. Run Molecule:

```bash
molecule test
```

This will run the molecule test, create a Docker container, run the role against it, run any associated `default` tests (see [molecule/default](./molecule/default) directory), and then destroy the container.

For more information on using Molecule, see the [Molecule documentation](https://molecule.readthedocs.io/).

## License

MIT

## Author Information

Created in 2020 by Michal Muransky
