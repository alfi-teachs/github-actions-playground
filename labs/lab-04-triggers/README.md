# Lab 04 – GitHub Actions Triggers

## Step 1: Understand Workflow Triggers

A workflow trigger is an event that starts a GitHub Actions workflow.

GitHub Actions supports different triggers:

1. `push` – Runs when code is pushed to a branch.
2. `pull_request` – Runs when a pull request is opened or updated.
3. `workflow_dispatch` – Allows manual execution from GitHub Actions.

## Step 2: Understand the Push Trigger

The `push` trigger starts the workflow when code is pushed to the specified branch.

Example:

```yaml
on:
  push:
    branches:
      - main
```

Explanation:
- `push` – Starts the workflow when code is pushed.
- `branches` – Specifies the branch to monitor.
- `main` – The workflow runs when changes are pushed to main.

## Step 3: Understand the Pull Request Trigger

The `pull_request` trigger starts the workflow when a pull request targets the specified branch.

Example:

```yaml
on:
  pull_request:
    branches:
      - main
```

Explanation:
- `pull_request` – Starts the workflow when a pull request event occurs.
- `branches` – Specifies the target branch.
- `main` – The pull request must target main.

## Step 4: Understand the Workflow Dispatch Trigger

The `workflow_dispatch` trigger allows us to manually start a workflow from the GitHub Actions tab.

Example:

```yaml
on:
  workflow_dispatch:
```

Explanation:
- `workflow_dispatch` – Enables manual workflow execution.
- We can start the workflow by clicking **Run workflow** in GitHub Actions.

## Step 5: Create the Workflow Directory and File

All GitHub Actions workflow files must be placed inside:

```text
.github/
└── workflows/
    └── lab-04-triggers.yaml
```

Create the file:

`lab-04-triggers.yaml`

## Step 6: Write the Complete Workflow

Add the following YAML configuration:

```yaml
name: Trigger Demo Workflow

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
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Display trigger message
        run: echo "The workflow was triggered successfully!"

      - name: Display event name
        run: 'echo "Event: $GITHUB_EVENT_NAME"'

      - name: Display branch name
        run: 'echo "Branch: $GITHUB_REF_NAME"'
```

## Step 7: Understand the Workflow Configuration

| YAML keyword | Purpose |
|---|---|
| `name` | Defines the workflow name |
| `on` | Defines workflow triggers |
| `push` | Runs when code is pushed |
| `pull_request` | Runs for pull request events |
| `workflow_dispatch` | Enables manual execution |
| `jobs` | Defines workflow jobs |
| `runs-on` | Specifies the runner |
| `steps` | Defines tasks inside a job |
| `actions/checkout@v4` | Checks out repository code |
| `GITHUB_EVENT_NAME` | Displays the event that triggered the workflow |
| `GITHUB_REF_NAME` | Displays the branch or reference name |

## Step 8: Save, Commit and Push the Workflow

Open Git Bash and navigate to the project directory.

```bash
cd ~/OneDrive/Desktop/lab/github-actions-playground
```

Check the repository status:

```bash
git status
```

Add and commit the workflow:

```bash
git add .
git commit -m "Add GitHub Actions trigger workflow"
git push origin main
```

## Step 9: Test the Push Trigger

1. Make a change in your repository.
2. Save the file.
3. Open Git Bash.
4. Execute:

```bash
git add .
git commit -m "Test push trigger"
git push origin main
```

5. Open your GitHub repository.
6. Click **Actions**.
7. Select **Trigger Demo Workflow**.
8. Open the latest workflow run.
9. Click the `trigger-demo` job.
10. Check the step logs.

Expected output:

```text
The workflow was triggered successfully!
Event: push
Branch: main
```

## Step 10: Test the Manual Trigger

1. Open your GitHub repository.
2. Click **Actions**.
3. Select **Trigger Demo Workflow**.
4. Click **Run workflow**.
5. Select the `main` branch.
6. Click the green **Run workflow** button.
7. Open the latest workflow run.
8. Click the `trigger-demo` job.
9. Check the step logs.

Expected output:

```text
The workflow was triggered successfully!
Event: workflow_dispatch
Branch: main
```

Important: `workflow_dispatch` is configured under the `on` section. It is not a job or a step. The `Display event name` step prints its value when you manually start the workflow.

## Step 11: Test the Pull Request Trigger

1. Open Git Bash in the project directory.
2. Create a new branch:

```bash
git checkout -b feature/test-pull-request
```

3. Make a change to a file and save it.
4. Add and commit the changes:

```bash
git add .
git commit -m "Test pull request trigger"
```

5. Push the new branch:

```bash
git push -u origin feature/test-pull-request
```

6. Open your GitHub repository.
7. Create a pull request from `feature/test-pull-request` into `main`.
8. Click **Create pull request**.
9. Open the **Actions** tab.
10. Check the latest workflow run and its job logs.

Expected event name:

```text
Event: pull_request
```

## Step 12: Compare the Three Triggers

| Trigger | When does it run? |
|---|---|
| `push` | When code is pushed to the specified branch |
| `pull_request` | When a pull request targets the specified branch |
| `workflow_dispatch` | When a user manually starts the workflow |

## Step 13: Important Points to Remember

1. A workflow trigger is an event that starts a workflow.
2. Triggers are configured under the `on` section.
3. Multiple triggers can be configured in one workflow.
4. `push` and `pull_request` are event-based triggers.
5. `workflow_dispatch` enables manual execution.
6. `GITHUB_EVENT_NAME` displays which event started the workflow.
7. `GITHUB_REF_NAME` displays the branch or reference name.
8. Workflow files must be stored inside `.github/workflows/`.

## Step 14: Lab Completion Checklist

- [x] Understood workflow triggers.
- [x] Configured the push trigger.
- [x] Configured the pull request trigger.
- [x] Configured the workflow dispatch trigger.
- [x] Created the trigger workflow YAML file.
- [x] Committed and pushed the workflow.
- [x] Tested the push trigger.
- [x] Tested the manual trigger.
- [x] Tested the pull request trigger practically.

**Lab 04 – GitHub Actions Triggers**
