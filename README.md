GitHub Actions Runner
=========

<a href="https://galaxy.ansible.com/monolithprojects/system_update"><img src="https://img.shields.io/ansible/quality/47118?style=flat&logo=ansible"/></a> 
<a href="https://galaxy.ansible.com/monolithprojects/system_update"><img src="https://img.shields.io/ansible/role/d/47118"/></a> 
<a href="https://github.com/MonolithProjects/ansible-github_actions_runner/blob/master/LICENSE"><img src="https://img.shields.io/github/license/MonolithProjects/ansible-github_actions_runner"/></a>

This role will deploy local GitHub Actions Runner.

**Note:**  
The role is in early development stage.  
Role is able to:
- install and cofigure local runner
- request reistration token
- register the runner to GitHub

Currently is missing:
- idempotency
- runner unregistration
- automated testing

Requirements
------------

System must have access to the packages repository (Internet, Red Hat Satellite, etc.).

CentOS/Fedora systems require EPEL repository.

`PERSONAL_ACCESS_TOKEN` variable needs to be exported to your environment.

Role Variables
--------------

This is a copy from `defaults/main.yml`

```yaml
# Directory where the local runner will be installed
runner_dir: "/opt/actions-runner"

# Version of the GitHub Actions Runner
runner_version: "2.165.2"

# Personal Access Token for your GitHub account
access_token: "{{ lookup('env', 'PERSONAL_ACCESS_TOKEN') }}"

# GitHub address
github_server: "https://github.com"

# GitHub account name
# github_account: "youruser"

# Github repository name
# github_repo: "yourrepo"
```

Example Playbook
----------------

Simple example.

```yaml
---
- name: Converge
  hosts: all
  become: yes
  vars:
    - runner_user: runner
    - github_account: example
    - github_repo: example
  roles:
    - role: monolithprojects/github_actions_runner
```

License
-------

MIT

Author Information
------------------

Created in 2020 by Michal Muransky