# How to contribute

Feel free to:

- [Create an issue](https://help.github.com/articles/creating-an-issue/)
- [Make a pull request](https://services.github.com/on-demand/github-cli/open-pull-request-github) into the `master` branch

Here is how you can help, a lot of steps are related to GitHub, not specifically my roles.

## 1. Create an issue

When you see some issue or have an idea for improvement, [create an issue](https://github.com/monolithprojects/ansible-github_actions_runner/issues).

## 2. Fork the project

Click on `fork` on the top-right corner and fork the repository.

## 3. Install pre-commit

Install [Pre-commit](https://pre-commit.com/#install) software

## 4. Make the changes

Do the changes in your own GitHub namespace.

## 5. Test the changes with Molecule

[Molecule](https://molecule.readthedocs.io/) is a testing framework for Ansible that we use to test the `monolithprojects.github_actions_runner` role.

### Prerequisites

- Python
- Docker
- Ansible
- Molecule

### Installation

1. Install Python, Docker, and Ansible if you haven't already.
2. Install Molecule and its Docker driver with pip:

```bash
pip install "molecule-plugins[docker]"
```

### Running Tests

1. Navigate to the role's directory:

```bash
cd path/to/monolithprojects.github_actions_runner
```

2. Set Environment variables

```bash
export PERSONAL_ACCESS_TOKEN=your_github_pat # Your Personal Access Token to Github
export GITHUB_ACCOUNT=your_account # Your Github Account
export GITHUB_ACCOUNT=your_repository # Github Repository where you want to setup the Runner
```

3. Run Molecule:

```bash
molecule test
```

This will run the molecule test, create a Docker container, run the role against it, run any associated `default` tests (see [molecule/default](../molecule/default) directory), and then destroy the container.

For more information on using Molecule, see the [Molecule documentation](https://molecule.readthedocs.io/).

## 6. Create a pull request

Please create a pull request into the `develop` branch. Here is [how to do it](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request-from-a-fork).

Please consider to [sponsor](https://github.com/sponsors/monolithprojects) me :-)
