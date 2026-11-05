# Lab 5 – GitHub Actions Variables and Secrets

## 1. Objective

Learn how to configure and use GitHub Repository Variables and Secrets in GitHub Actions workflows.

### Topics covered
- Repository Variables
- Repository Secrets
- Accessing variables using `${{ vars.NAME }}`
- Accessing secrets using `${{ secrets.NAME }}`
- Passing secrets through environment variables
- Testing variables and secrets in GitHub Actions

## 2. Project Structure

```text
github-actions-playground/
│
├── .github/
│   └── workflows/
│       ├── welcome.yaml
│       ├── lab-04-triggers.yaml
│       └── lab-05-variables.yaml
│
└── labs/
    └── lab-05-variables-secrets/
        └── README.md
```

## 3. Understanding Variables and Secrets

| Variables | Secrets |
|---|---|
| Store non-sensitive information | Store sensitive information |
| Application name | Passwords |
| Environment name | API tokens |
| AWS region | AWS credentials |
| Access using `vars` | Access using `secrets` |

### Syntax

Access a repository variable:

```yaml
${{ vars.APP_NAME }}
```

Access a repository secret:

```yaml
${{ secrets.DEMO_PASSWORD }}
```

## 4. Part 1 – Create a Repository Variable

### Step 1: Open GitHub repository settings

1. Open the GitHub repository.
2. Click Settings.
3. Select Secrets and variables.
4. Select Actions.
5. Open the Variables tab.
6. Click New repository variable.

### Step 2: Create the variable

| Field | Value |
|---|---|
| Name | APP_NAME |
| Value | devops-demo |

Click Add variable.

### Step 3: Verify the variable

Check that `APP_NAME` appears in the Repository Variables list.

## 5. Part 1 – Create the GitHub Actions Workflow

### Step 1: Create the workflow file

Create:

`.github/workflows/lab-05-variables.yaml`

### Step 2: Add the YAML configuration

```yaml
name: Variables Demo

on:
  push:
    branches:
      - main

  workflow_dispatch:

jobs:
  variables-demo:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Display application name
        run: 'echo "Application Name: ${{ vars.APP_NAME }}"'

      - name: Display environment
        run: 'echo "Environment: Development"'
```

### Step 3: Understand the workflow

1. `name` – Defines the workflow name.
2. `on` – Defines when the workflow runs.
3. `push` – Runs when code is pushed to main.
4. `workflow_dispatch` – Allows manual execution.
5. `runs-on` – Uses a GitHub-hosted Ubuntu runner.
6. `actions/checkout@v4` – Checks out the repository.
7. `${{ vars.APP_NAME }}` – Reads the repository variable.

### Step 4: Commit and push

```bash
git status

git add .github/workflows/lab-05-variables.yaml

git commit -m "Add GitHub Actions variables and secrets lab"

git push origin main
```

### Step 5: Test the workflow

1. Open the GitHub repository.
2. Click Actions.
3. Select Variables Demo.
4. Click Run workflow.
5. Select main.
6. Click Run workflow.
7. Open the workflow run and inspect the job logs.

### Expected output

```text
Application Name: devops-demo
Environment: Development
```

**Result:** Successfully accessed the GitHub Repository Variable through GitHub Actions.

## 6. Part 2 – Create a GitHub Repository Secret

### Step 1: Open GitHub settings

1. Open Settings.
2. Select Secrets and variables.
3. Select Actions.
4. Open the Secrets tab.
5. Click New repository secret.

### Step 2: Create a dummy secret

| Field | Value |
|---|---|
| Name | DEMO_PASSWORD |
| Secret | MyTestPassword123! |

Use a dummy password for this exercise, not a real password.

Click Add secret.

### Step 3: Verify the secret

Check that `DEMO_PASSWORD` appears in the Repository Secrets list.

Remember: GitHub hides secret values after they are saved.

## 7. Part 2 – Access the Secret in GitHub Actions

Add the following step below the Display environment step in the existing workflow:

```yaml
      - name: Check secret availability
        run: |
          if [ -n "$DEMO_PASSWORD" ]; then
            echo "Secret is configured successfully"
          else
            echo "Secret is missing"
            exit 1
          fi
        env:
          DEMO_PASSWORD: ${{ secrets.DEMO_PASSWORD }}
```

### Understanding this configuration

- `env` passes the secret into the step as an environment variable.
- `${{ secrets.DEMO_PASSWORD }}` retrieves the configured repository secret.
- `$DEMO_PASSWORD` accesses the value inside the shell script.
- `-n` checks whether the value is non-empty.
- The workflow does not print the actual password.

**Important:** Never use `echo` to display the actual secret value.

### Step 4: Test the secret

After adding the step:

1. Save the YAML file.
2. Commit and push the change.
3. Open GitHub Actions.
4. Run Variables Demo.
5. Open the job logs.
6. Check the secret availability step.

Expected output:

```text
Secret is configured successfully
```

## 8. Git Commands Reference

| Command | Purpose |
|---|---|
| `git status` | Check repository changes |
| `git add .` | Stage changes |
| `git add filename` | Stage a specific file |
| `git commit -m "message"` | Commit changes |
| `git push origin main` | Push changes to GitHub |
| `git pull origin main` | Pull latest changes |

## 9. Important Security Practices

1. Never hardcode passwords in workflow YAML files.
2. Store confidential information in GitHub Secrets.
3. Do not print secret values in workflow logs.
4. Use repository variables for non-sensitive configuration.
5. Avoid committing passwords or credentials to GitHub.

## 10. Lab Completion Checklist

- [x] Created repository variable `APP_NAME`.
- [x] Created `lab-05-variables.yaml`.
- [x] Accessed the repository variable.
- [x] Tested the workflow successfully.
- [x] Created repository secret `DEMO_PASSWORD`.
- [ ] Accessed the secret securely in the workflow.
- [ ] Tested the secret availability step.
- [ ] Committed and pushed the final workflow.
- [ ] Completed the lab documentation.

## 11. Final Learning Outcome

After completing this lab, I can configure GitHub Repository Variables and Secrets, access them inside GitHub Actions workflows, and use environment variables to handle sensitive information securely.
