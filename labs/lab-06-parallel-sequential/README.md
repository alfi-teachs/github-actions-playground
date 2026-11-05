# Lab 6 – Parallel and Sequential Execution in GitHub Actions

## 1. Objective

Learn how to execute multiple jobs in GitHub Actions using:

- Parallel execution
- Sequential execution
- Job dependencies using `needs`

## 2. Project Structure

```text
github-actions-playground/
│
├── .github/
│   └── workflows/
│       └── lab-06-parallel-sequential.yaml
│
└── labs/
    └── lab-06-parallel-sequential/
        └── README.md
```

## 3. Understanding Parallel Execution

Parallel execution means multiple jobs can run at the same time.

By default, GitHub Actions runs independent jobs in parallel.

### Step 1: Create the workflow file

Create:

`.github/workflows/lab-06-parallel-sequential.yaml`

### Step 2: Add the workflow YAML

```yaml
name: Parallel and Sequential Jobs Demo

on:
  push:
    branches:
      - main

  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Build application
        run: echo "Building application..."

      - name: Build completed
        run: echo "Build completed successfully!"

  test:
    runs-on: ubuntu-latest
    steps:
      - name: Run tests
        run: echo "Running application tests..."

      - name: Tests completed
        run: echo "Tests completed successfully!"
```

### Step 3: Understand the YAML

| Keyword | Explanation |
|---|---|
| `name` | Workflow name |
| `on` | Defines workflow triggers |
| `push` | Runs workflow when code is pushed |
| `workflow_dispatch` | Allows manual workflow execution |
| `jobs` | Defines multiple jobs |
| `build` | First job |
| `test` | Second job |
| `runs-on` | Specifies the runner |
| `steps` | Tasks inside a job |
| `run` | Executes a shell command |

### Step 4: Commit and push

```bash
git add .github/workflows/lab-06-parallel-sequential.yaml

git commit -m "Add parallel jobs workflow"

git push origin main
```

### Step 5: Test parallel execution

1. Open the GitHub repository.
2. Click Actions.
3. Select Parallel and Sequential Jobs Demo.
4. Click Run workflow.
5. Select the main branch.
6. Click Run workflow.
7. Open the workflow run.

Expected jobs:

- Build
- Test

Both jobs can execute independently without waiting for each other.

**Result:** Successfully tested parallel job execution.

## 4. Understanding Sequential Execution

Sequential execution means one job must wait for another job to complete successfully.

GitHub Actions uses the `needs` keyword to establish job dependencies.

### Step 1: Modify the existing workflow

Open:

`.github/workflows/lab-06-parallel-sequential.yaml`

Add `needs: build` under the test job.

### Step 2: Updated YAML

```yaml
name: Parallel and Sequential Jobs Demo

on:
  push:
    branches:
      - main

  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Build application
        run: echo "Building application..."

      - name: Build completed
        run: echo "Build completed successfully!"

  test:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Run tests
        run: echo "Running application tests..."

      - name: Tests completed
        run: echo "Tests completed successfully!"
```

### Step 3: Understand `needs`

```yaml
test:
  needs: build
```

Explanation:

- `test` is the dependent job.
- `needs` defines the dependency.
- `build` is the job that must finish first.
- The test job starts after the build job completes successfully.

### Step 4: Commit and push the changes

```bash
git add .github/workflows/lab-06-parallel-sequential.yaml

git commit -m "Add sequential job dependency using needs"

git push origin main
```

### Step 5: Test sequential execution

1. Open GitHub.
2. Go to Actions.
3. Select Parallel and Sequential Jobs Demo.
4. Click Run workflow.
5. Select main.
6. Click Run workflow.
7. Open the latest workflow run.

Expected execution:

1. Build job executes first.
2. Build job completes successfully.
3. Test job starts.
4. Test job completes successfully.

**Result:** Successfully tested sequential job execution using `needs`.

## 5. Parallel vs Sequential Execution

| Feature | Parallel | Sequential |
|---|---|---|
| Execution | At the same time | One after another |
| Dependency | No dependency required | Uses `needs` |
| Waiting | Jobs don't wait for each other | Dependent jobs wait |
| Example | Build and Test independently | Build → Test → Deploy |
| Failure | Independent jobs can still run | Dependent jobs are skipped by default if a required job fails |

## 6. Important GitHub Actions Commands and Keywords

| Keyword / Command | Purpose |
|---|---|
| `jobs` | Defines workflow jobs |
| `steps` | Defines tasks inside a job |
| `runs-on` | Selects the runner |
| `needs` | Defines job dependencies |
| `run` | Executes shell commands |
| `workflow_dispatch` | Enables manual execution |
| `git add` | Stages changes |
| `git commit` | Saves changes locally |
| `git push` | Pushes changes to GitHub |

## 7. Interview Questions

**Q1. What is parallel execution in GitHub Actions?**

Parallel execution means multiple independent jobs run at the same time by default.

**Q2. What is sequential execution?**

Sequential execution means jobs execute one after another based on dependencies.

**Q3. What is the purpose of `needs`?**

The `needs` keyword defines a dependency between jobs. A job waits for its required job to complete successfully.

**Q4. What happens if a job defined in `needs` fails?**

The dependent job is skipped by default.

**Q5. Do jobs run in parallel by default?**

Yes. Independent jobs run in parallel by default.

## 8. Lab Completion Checklist

- [x] Created Lab 6 folder.
- [x] Created README.md.
- [x] Created workflow YAML file.
- [x] Created multiple jobs.
- [x] Tested parallel execution.
- [x] Understood the `needs` keyword.
- [x] Added `needs: build`.
- [x] Tested sequential execution.
- [x] Verified both jobs completed successfully.
- [x] Committed and pushed workflow changes.
- [x] Documented practical steps.

## 9. Final Learning Outcome

After completing this lab, I can create multiple jobs in GitHub Actions, understand parallel and sequential execution, configure job dependencies using `needs`, and verify job execution through GitHub Actions logs.