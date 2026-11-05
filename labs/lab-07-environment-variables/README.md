# LAB 7 – GitHub Actions Environment Variables, GITHUB_ENV and GITHUB_OUTPUT

## 1. Objective

The objective of this lab is to understand how to pass environment variables and application version information between steps and jobs in GitHub Actions.

In this lab, we will learn:

1. How to use `GITHUB_ENV` to share variables between steps.
2. How to use `GITHUB_OUTPUT` to create step outputs.
3. How to pass outputs between different jobs.
4. How to use the `needs` keyword.
5. How to pass an application version from a Build job to a Deploy job.

## 2. Prerequisites

Before starting this lab, make sure you have:

- Git installed.
- Git Bash.
- Visual Studio Code.
- A GitHub account.
- An existing GitHub Actions repository.
- Basic knowledge of YAML.
- Basic knowledge of Git commands.

Repository:

`https://github.com/alfi-teachs/github-actions-playground`

Local project directory:

`~/OneDrive/Desktop/lab/github-actions-playground`

## 3. Create the Lab 7 Folder

### Step 1: Open Git Bash

Navigate to the project directory:

```bash
cd ~/OneDrive/Desktop/lab/github-actions-playground
```

### Step 2: Create the Lab 7 folder

```bash
mkdir -p labs/lab-07-environment-variables
```

### Step 3: Create README.md

```bash
touch labs/lab-07-environment-variables/README.md
```

### Step 4: Create the workflow YAML file

```bash
touch .github/workflows/lab-07-environment-variables.yaml
```

### Step 5: Verify the files

```bash
ls labs/lab-07-environment-variables
```

Expected output:

```text
README.md
```

Check the workflow folder:

```bash
ls .github/workflows/
```

Expected output includes:

```text
lab-04-triggers.yaml
lab-05-variables.yaml
lab-06-parallel-sequential.yaml
lab-07-environment-variables.yaml
welcome.yaml
```

## 4. Exercise 1 – GITHUB_ENV

### What is GITHUB_ENV?

`GITHUB_ENV` is a special file provided by GitHub Actions.

It allows us to create or update an environment variable in one step and make it available to subsequent steps in the same job.

### Step 1: Open the workflow file

Open:

`.github/workflows/lab-07-environment-variables.yaml`

### Step 2: Add the following YAML

```yaml
name: Environment Variables Demo

on:
  push:
    branches:
      - main

  workflow_dispatch:

jobs:
  environment-variables-demo:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set environment variable
        run: |
          echo "APP_ENV=Development" >> "$GITHUB_ENV"

      - name: Read application environment
        run: 'echo "Application Environment: $APP_ENV"'
```

### Step 3: Understand the YAML

| Keyword | Explanation |
|---|---|
| `name` | Workflow name |
| `on` | Defines workflow triggers |
| `push` | Runs when code is pushed |
| `workflow_dispatch` | Allows manual execution |
| `jobs` | Contains workflow jobs |
| `runs-on` | Defines the runner |
| `steps` | Contains individual tasks |
| `GITHUB_ENV` | Shares variables with subsequent steps in the same job |

### Step 4: Understand the command

```bash
echo "APP_ENV=Development" >> "$GITHUB_ENV"
```

This writes the variable `APP_ENV` with the value `Development` into the special GitHub Actions environment file.

The next step accesses it using:

```bash
$APP_ENV
```

Important: The variable is available to subsequent steps, not to the step that writes it.

### Step 5: Save the file

Press:

`Ctrl + S`

## 5. Exercise 2 – GITHUB_OUTPUT

### What is GITHUB_OUTPUT?

`GITHUB_OUTPUT` is used to create outputs from a particular workflow step.

Another step in the same job can retrieve that output using the step ID.

### Step 1: Add another job

Add this job below `environment-variables-demo`, under `jobs:`.

```yaml
  output-demo:
    runs-on: ubuntu-latest

    steps:
      - name: Create application version
        id: app
        run: |
          echo "version=2.0.0" >> "$GITHUB_OUTPUT"

      - name: Display application version
        run: 'echo "Application Version: ${{ steps.app.outputs.version }}"'
```

### Step 2: Understand the keywords

| Keyword | Explanation |
|---|---|
| `id: app` | Identifies the step |
| `GITHUB_OUTPUT` | Creates an output for the step |
| `version=2.0.0` | Output name and value |
| `steps` | Accesses information from workflow steps |
| `outputs` | Retrieves the output value |

### Step 3: Understand the output expression

```yaml
${{ steps.app.outputs.version }}
```

Breakdown:

- `steps` – refers to workflow steps.
- `app` – refers to the step ID.
- `outputs` – refers to the output created by that step.
- `version` – the output name.

### Expected output

```text
Application Version: 2.0.0
```

## 6. Exercise 3 – Passing Outputs Between Jobs

This exercise demonstrates how to pass an application version from a Build job to a Deploy job.

### Real-world DevOps scenario

Imagine an application has multiple versions.

The Build job creates a particular version, for example `3.0.0`.

The Deploy job needs to know which version was created by the Build job.

We can pass that information using:

- `GITHUB_OUTPUT`
- Job-level `outputs`
- `needs`

### Step 1: Create the Build job

Add the following below `output-demo`:

```yaml
  build-demo:
    runs-on: ubuntu-latest

    outputs:
      app_version: ${{ steps.version.outputs.app_version }}

    steps:
      - name: Set application version
        id: version
        run: |
          echo "app_version=3.0.0" >> "$GITHUB_OUTPUT"

      - name: Display application version
        run: 'echo "Application Version: ${{ steps.version.outputs.app_version }}"'
```

### Step 2: Understand the Build job

```yaml
outputs:
  app_version: ${{ steps.version.outputs.app_version }}
```

This exposes the output created by the step to other jobs.

The output is named `app_version`.

The step ID is `version`.

### Step 3: Create the Deploy job

Add the following below `build-demo`:

```yaml
  deploy:
    runs-on: ubuntu-latest

    needs: build-demo

    steps:
      - name: Display application version
        run: 'echo "Deploying Application Version: ${{ needs.build-demo.outputs.app_version }}"'
```

### Step 4: Understand the needs keyword

```yaml
needs: build-demo
```

This means:

1. Deploy waits for Build to finish.
2. If Build succeeds, Deploy can run.
3. Deploy can access the outputs exposed by Build.

### Step 5: Understand the output expression

```yaml
${{ needs.build-demo.outputs.app_version }}
```

Breakdown:

- `needs` – accesses a job that this job depends on.
- `build-demo` – the job ID that creates the output.
- `outputs` – accesses the job outputs.
- `app_version` – the output name.

### Expected output

```text
Deploying Application Version: 3.0.0
```

## 7. Complete Workflow YAML

The complete file should contain all four jobs.

File:

`.github/workflows/lab-07-environment-variables.yaml`

```yaml
name: Environment Variables Demo

on:
  push:
    branches:
      - main

  workflow_dispatch:

jobs:
  environment-variables-demo:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set environment variable
        run: |
          echo "APP_ENV=Development" >> "$GITHUB_ENV"

      - name: Read application environment
        run: 'echo "Application Environment: $APP_ENV"'

  output-demo:
    runs-on: ubuntu-latest

    steps:
      - name: Create application version
        id: app
        run: |
          echo "version=2.0.0" >> "$GITHUB_OUTPUT"

      - name: Display application version
        run: 'echo "Application Version: ${{ steps.app.outputs.version }}"'

  build-demo:
    runs-on: ubuntu-latest

    outputs:
      app_version: ${{ steps.version.outputs.app_version }}

    steps:
      - name: Set application version
        id: version
        run: |
          echo "app_version=3.0.0" >> "$GITHUB_OUTPUT"

      - name: Display application version
        run: 'echo "Application Version: ${{ steps.version.outputs.app_version }}"'

  deploy:
    runs-on: ubuntu-latest

    needs: build-demo

    steps:
      - name: Display application version
        run: 'echo "Deploying Application Version: ${{ needs.build-demo.outputs.app_version }}"'
```

## 8. Validate and Push the Workflow

### Step 1: Check the workflow file

```bash
cat .github/workflows/lab-07-environment-variables.yaml
```

### Step 2: Check for whitespace errors

```bash
git diff --check
```

### Step 3: Check Git status

```bash
git status
```

### Step 4: Add the workflow file

```bash
git add .github/workflows/lab-07-environment-variables.yaml
```

### Step 5: Commit the workflow

```bash
git commit -m "Add GitHub Actions environment variables lab"
```

### Step 6: Push to GitHub

```bash
git push origin main
```

## 9. Test the Workflow in GitHub

### Step 1

Open your GitHub repository.

### Step 2

Click the Actions tab.

### Step 3

Select:

`Environment Variables Demo`

### Step 4

Open the latest workflow execution.

### Step 5

Check all four jobs.

Expected results:

| Job | Expected output |
|---|---|
| environment-variables-demo | Application Environment: Development |
| output-demo | Application Version: 2.0.0 |
| build-demo | Application Version: 3.0.0 |
| deploy | Deploying Application Version: 3.0.0 |

The `build-demo` and `deploy` jobs are connected through `needs`.

The other jobs are independent and can execute in parallel.

## 10. Difference Between GITHUB_ENV and GITHUB_OUTPUT

| GITHUB_ENV | GITHUB_OUTPUT |
|---|---|
| Used to share environment variables | Used to create step outputs |
| Available to subsequent steps in the same job | Available through step output references |
| Accessed using `$VARIABLE_NAME` | Accessed using `steps.id.outputs.name` |
| Does not automatically share across jobs | Can be exposed through job outputs |
| Useful for passing configuration between steps | Useful for passing values between steps and jobs |

## 11. Common Errors and Troubleshooting

### Error 1: Incorrect YAML indentation

YAML depends on correct indentation.

Make sure:

- All jobs are aligned under `jobs:`.
- `steps` is inside the correct job.
- `outputs` is inside the job.
- Step names and run commands have matching indentation.

### Error 2: Output is empty

Check that the step has an ID:

```yaml
id: version
```

Check that the output name matches:

```yaml
echo "app_version=3.0.0" >> "$GITHUB_OUTPUT"
```

Check the reference:

```yaml
${{ steps.version.outputs.app_version }}
```

### Error 3: Deploy runs before Build

Make sure the Deploy job contains:

```yaml
needs: build-demo
```

### Error 4: Job output is empty

Check the job-level mapping:

```yaml
outputs:
  app_version: ${{ steps.version.outputs.app_version }}
```

Also check that the step ID and output name match.

### Error 5: Workflow does not trigger

Check:

- The YAML file is inside `.github/workflows/`.
- The workflow file has a `.yaml` or `.yml` extension.
- The branch name is correct.
- The file has been pushed to GitHub.

## 12. Important Interview Questions

**Q1. What is GITHUB_ENV?**

GITHUB_ENV is a special file used in GitHub Actions to share environment variables between subsequent steps within the same job.

**Q2. What is GITHUB_OUTPUT?**

GITHUB_OUTPUT is used to create outputs from a workflow step. These outputs can be accessed by other steps or exposed to other jobs.

**Q3. What is the difference between GITHUB_ENV and GITHUB_OUTPUT?**

GITHUB_ENV shares environment variables between steps in the same job, whereas GITHUB_OUTPUT creates step outputs that can also be exposed between jobs.

**Q4. What is the purpose of needs?**

The needs keyword defines job dependencies. It makes one job wait for another job to complete successfully before running.

**Q5. How do you pass an application version from Build to Deploy?**

We create a step output using GITHUB_OUTPUT, expose it through job-level outputs, and retrieve it in the Deploy job using needs.

**Q6. Can GITHUB_ENV share variables between separate jobs?**

No. GITHUB_ENV only shares variables with subsequent steps in the same job. For job-to-job communication, we can use job outputs.

**Q7. What happens if the Build job fails?**

By default, a dependent Deploy job using needs will be skipped if Build fails.

**Q8. Why do we use step IDs?**

Step IDs allow us to identify a particular step and retrieve its outputs using expressions.

## 13. Lab Completion Checklist

- [x] Created Lab 7 folder.
- [x] Created README.md.
- [x] Created workflow YAML.
- [x] Tested GITHUB_ENV.
- [x] Tested GITHUB_OUTPUT between steps.
- [x] Created Build job output.
- [x] Passed application version to Deploy job.
- [x] Used needs keyword.
- [x] Verified successful workflow execution.
- [x] Verified expected application versions.
- [ ] Completed and saved this README documentation.
- [ ] Committed and pushed the README documentation.

## 14. Important Concepts to Remember

1. `GITHUB_ENV` – Share environment variables between steps in the same job.
2. `GITHUB_OUTPUT` – Create an output from a step.
3. `steps.id.outputs.name` – Retrieve a step output.
4. `jobs.outputs` – Expose a step output at the job level.
5. `needs` – Define job dependencies.
6. `needs.job_id.outputs.name` – Retrieve an output from a dependent job.

**Lab 7 completed: GitHub Actions Environment Variables, Step Outputs and Job Outputs.**