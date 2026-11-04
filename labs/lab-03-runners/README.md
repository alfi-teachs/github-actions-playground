# Lab 3 – Understanding GitHub Actions Runners

## Objective
Understand how GitHub-hosted runners execute workflow jobs and how to inspect the runner environment.

## 1. What is a Runner?

A runner is a machine or environment that executes a GitHub Actions job.

For example:

```yaml
runs-on: ubuntu-latest
```

This tells GitHub Actions to use a GitHub-hosted Ubuntu runner.

## 2. Commands to Explore the Runner

| Command | Purpose |
|---|---|
| `uname -a` | Displays operating system and kernel information |
| `pwd` | Displays the current working directory |
| `ls -la` | Lists files, including hidden files |
| `whoami` | Displays the current user |
| `df -h` | Displays disk space information |

## 3. Checkout Action

Before accessing repository files, we need to check out the repository onto the runner.

```yaml
- name: Checkout repository
  uses: actions/checkout@v4
```

The checkout action downloads the repository contents into the runner's workspace.

Without checkout, the runner does not automatically have our project files.

## 4. Important Concepts

- GitHub-hosted runners are managed by GitHub.
- Each job runs on a runner.
- The runner executes the steps defined in the job.
- `runs-on` specifies the runner environment.
- `actions/checkout` makes repository files available to the workflow.

## 5. What I Learned

- How to identify the runner operating system.
- How to inspect the runner's working directory.
- How to list files in the runner environment.
- Why the checkout action is required.
- How GitHub-hosted runners execute workflow jobs.

## Outcome

Learned how GitHub Actions runners work and how to inspect their environment using Linux commands.