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

## Step 11: Practical Exercise – Create a Feature Branch

In this exercise, we create a separate branch to test the `pull_request` trigger.

Open Git Bash and navigate to the project directory:

```bash
cd ~/OneDrive/Desktop/lab/github-actions-playground
```

Create a new branch:

```bash
git checkout -b feature/test-pull-request
```

Verify the branch:

```bash
git branch
```

Expected output:

```text
* feature/test-pull-request
  main
```

## Step 12: Create a Test File

Create a new file to simulate a code change:

```bash
echo "Testing GitHub Actions pull request trigger" > pull-request-test.txt
```

Check the repository status:

```bash
git status
```

The new file should appear under Untracked files.

## Step 13: Stage and Commit Changes

Stage all changes:

```bash
git add .
```

Verify:

```bash
git status
```

Commit the changes:

```bash
git commit -m "Test pull request trigger"
```

## Step 14: Push the Feature Branch to GitHub

Push the newly created branch:

```bash
git push -u origin feature/test-pull-request
```

Explanation:

- `git push` – Uploads local commits to GitHub.
- `-u` – Sets the upstream tracking branch.
- `origin` – Name of the remote repository.
- `feature/test-pull-request` – Branch being pushed.

## Step 15: Create a Pull Request on GitHub

1. Open the GitHub repository.
2. Click the **Pull requests** tab.
3. Click **New pull request**.
4. Select the base branch as `main`.
5. Select the compare branch as `feature/test-pull-request`.
6. Review the changed files.
7. Click **Create pull request**.

Branch comparison:

| Field | Branch |
|---|---|
| Base | `main` |
| Compare | `feature/test-pull-request` |

## Step 16: Verify the Pull Request Workflow

1. Open the **Actions** tab.
2. Select **Trigger Demo Workflow**.
3. Open the latest workflow run.
4. Click the `trigger-demo` job.
5. Expand the **Display event name** step.

Expected output:

```text
Event: pull_request
```

This confirms that the pull request trigger executed successfully.

## Step 17: Merge the Pull Request

After reviewing the changes and checking that all required checks passed:

1. Return to the Pull Request page.
2. Click **Merge pull request**.
3. Click **Confirm merge**.
4. Verify that the pull request is marked as merged.

Important: Merging a pull request updates the target branch (`main`). This can also trigger workflows configured with a `push` event.

## Step 18: Synchronize the Local Main Branch

After merging, switch to your local main branch:

```bash
git checkout main
```

Download the latest changes from GitHub:

```bash
git pull origin main
```

Verify the repository:

```bash
git status
```

Expected result:

```text
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

## Step 19: Final Verification

Check the Actions logs and confirm that the event name is:

```text
Event: pull_request
```

Also verify the push and manual trigger outputs:

```text
Event: push
```

```text
Event: workflow_dispatch
```

## Step 20: Lab Completion Checklist

- [x] Created a feature branch.
- [x] Created a test file.
- [x] Staged and committed changes.
- [x] Pushed the feature branch to GitHub.
- [x] Created a pull request targeting main.
- [x] Verified the pull request workflow.
- [x] Merged the pull request.
- [x] Synchronized the local main branch.
- [x] Verified all three workflow triggers.

**Lab 04 – GitHub Actions Triggers: Completed**
