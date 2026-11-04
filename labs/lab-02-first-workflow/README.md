# Lab 2 – First GitHub Actions Workflow

## Objective
Create and execute a basic GitHub Actions workflow that displays messages when code is pushed to the `main` branch.

## Workflow File

Location:

```text
.github/workflows/welcome.yaml
```

## Workflow Code

```yaml
name: My First GitHub Actions Workflow

on:
  push:
    branches:
      - main

jobs:
  welcome:
    runs-on: ubuntu-latest

    steps:
      - name: Display welcome message
        run: echo "Welcome to GitHub Actions!"

      - name: Display project message
        run: echo "My first automation is running successfully!"
```

## Explanation

| YAML keyword | Purpose |
|---|---|
| `name` | Gives the workflow a name |
| `on` | Defines the event that triggers the workflow |
| `push` | Triggers the workflow when code is pushed |
| `branches` | Specifies which branch triggers it |
| `jobs` | Defines the jobs in the workflow |
| `welcome` | Name of the job |
| `runs-on` | Specifies the runner environment |
| `steps` | Contains tasks executed by the job |
| `run` | Executes a shell command |

## Execution Flow

1. We make a change to the project.
2. We stage and commit the change using Git.
3. We push the commit to the `main` branch.
4. GitHub detects the push event.
5. GitHub Actions starts the workflow.
6. The Ubuntu runner executes the steps.
7. The messages appear in the workflow logs.

## Git Commands

```bash
git add .
git commit -m "Add first GitHub Actions workflow"
git push
```

## Verification

1. Open the GitHub repository.
2. Select the **Actions** tab.
3. Open the workflow run.
4. Select the `welcome` job.
5. Check the output of both steps.

## Expected Output

```text
Welcome to GitHub Actions!
My first automation is running successfully!
```

## What I Learned

- How to create a workflow YAML file.
- How to configure a push trigger.
- How to define a job and its steps.
- How to execute shell commands using `run`.
- How to view workflow execution logs.

## Outcome

Successfully created a basic GitHub Actions workflow triggered by a push to the `main` branch.