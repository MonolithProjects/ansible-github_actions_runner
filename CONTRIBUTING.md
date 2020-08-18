# How to contribute

Feel free to:

- [Make an issue](https://help.github.com/articles/creating-an-issue/)
- [Make a pull request](https://services.github.com/on-demand/github-cli/open-pull-request-github) into the `develop` branch

Here is how you can help, a lot of steps are related to GitHub, not specifically my roles.

## 1. Create an issue

When you see some issue or have an idea for improvement, [create an issue](https://github.com/monolithprojects/ansible-github_actions_runner/issues).

## 2. Fork the project

Click on `fork` on the top-right corner and fork the repository.

## 3. Make the changes

Do dthe changes in your own GitHub namespace.

## 4. Test the changes

**Note:** I will test the role again, so you don't need to do that if you don't want to.

Install [molecule](https://molecule.readthedocs.io/en/stable/) and run the test:

```bash
pip install molecule ansible-lint docker
cd ansible-github_actions_runner
molecule test
```

## 5. Create a pull request

Please create a pull request into the `develop` branch. Here is [how to do it](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request-from-a-fork).


Please consider to [sponsor](https://github.com/sponsors/monolithprojects) me :-)