# Lab 4 – GitHub Actions Triggers

## Objective
Understand how to trigger GitHub Actions workflows using different GitHub events.

## 1. What is a Trigger?

A trigger is an event that tells GitHub Actions when to start a workflow.

We configure triggers using the `on` keyword in a workflow YAML file.

## 2. Common GitHub Actions Triggers

| Trigger | Purpose |
|---|---|
| `push` | Runs when code is pushed to a repository |
| `pull_request` | Runs when a pull request event occurs |
| `workflow_dispatch` | Allows us to start a workflow manually |
| `schedule` | Runs a workflow at scheduled times |

## 3. Push Trigger

Runs the workflow when code is pushed to the `main` branch.

```yaml
on:
  push:
    branches:
      - main
```

## 4. Pull Request Trigger

Runs the workflow when a pull request targets the `main` branch.

```yaml
on:
  pull_request:
    branches:
      - main
```

## 5. Manual Trigger

Allows us to start a workflow manually from the GitHub Actions tab.

```yaml
on:
  workflow_dispatch:
```

## 6. Combining Triggers

We can configure multiple triggers in the same workflow.

```yaml
name: Trigger Demo

on:
  push:
    branches:
      - main

  pull_request:
    branches:
      - main

  workflow_dispatch:

jobs:
  trigger-demo:
    runs-on: ubuntu-latest

    steps:
      - name: Display trigger message
        run: echo "The workflow was triggered successfully!"
```

## 7. What I Learned

- What a workflow trigger is.
- How the `push` trigger works.
- How the `pull_request` trigger works.
- How to manually start a workflow using `workflow_dispatch`.
- How to configure multiple triggers in one workflow.

## Outcome

Learned how to configure different events to start GitHub Actions workflows.