# GitHub Actions – Beginner to Advanced

## Project Overview

This project is designed to learn GitHub Actions from beginner to advanced level through practical, hands-on labs.

We will build an interactive webpage and gradually integrate it with:

- Git and GitHub
- GitHub Actions
- Docker
- Docker Hub
- CI/CD pipelines
- Automated testing
- Security scanning
- Application deployment

**Learning approach:** Understand the concept → Write the code → Execute → Verify the output → Troubleshoot.

---

# PART 1 – BASIC UNDERSTANDING

## 1. What is GitHub Actions?

GitHub Actions is an automation tool provided by GitHub.

It allows us to automatically perform tasks whenever a particular event happens in our repository.

For example:

1. A developer modifies application code.
2. The developer pushes the changes to GitHub.
3. GitHub detects the push event.
4. GitHub Actions triggers a workflow.
5. The workflow executes the configured jobs and steps.

**Example:** Automatically build and test an application whenever new code is pushed to GitHub.

## 2. Important GitHub Actions Concepts

### A. Event

An event is something that happens in a GitHub repository and can trigger a workflow.

Examples:

- `push` – When code is pushed to GitHub.
- `pull_request` – When a pull request is opened or updated.
- `workflow_dispatch` – Allows a user to manually start a workflow.
- `schedule` – Runs a workflow according to a scheduled time.

### B. Workflow

A workflow is a complete automation process defined in a YAML file.

A repository can contain multiple workflows.

Example:

- Build workflow
- Test workflow
- Security scanning workflow
- Deployment workflow

Workflow files are stored inside:

`.github/workflows/`

### C. Job

A job is a group of steps that execute on a runner.

A workflow can contain one or multiple jobs.

Example:

- Build job
- Test job
- Deploy job

By default, jobs run in parallel unless dependencies are configured using the `needs` keyword.

### D. Step

A step is an individual task or action inside a job.

For example, a Build job can contain:

1. Checkout source code.
2. Install dependencies.
3. Build the application.

Steps inside the same job execute sequentially by default.

### E. Runner

A runner is the machine or execution environment that executes a GitHub Actions job.

Two common types:

**1. GitHub-hosted runner**

GitHub provides and manages the machine.

Example:

`runs-on: ubuntu-latest`

**2. Self-hosted runner**

A machine configured and maintained by the user or organization to execute GitHub Actions jobs.

### F. YAML

YAML stands for YAML Ain't Markup Language.

It is a human-readable format used to configure GitHub Actions workflows.

Example:

```yaml
name: My First Workflow

on:
  push:
    branches:
      - main

jobs:
  welcome:
    runs-on: ubuntu-latest

    steps:
      - name: Display message
        run: echo "Hello GitHub Actions"
```

---

# PART 2 – UNDERSTANDING THE WORKFLOW HIERARCHY

```text
EVENT
  |
  |--- Push
  |
  v
WORKFLOW
  |
  |--- JOB 1: BUILD
  |       |
  |       |--- Step 1: Checkout code
  |       |--- Step 2: Install dependencies
  |       |--- Step 3: Build application
  |
  |--- JOB 2: TEST
  |       |
  |       |--- Step 1: Run unit tests
  |       |--- Step 2: Display test results
  |
  v
RUNNERS
```

Remember:

- Event triggers the workflow.
- Workflow contains jobs.
- Jobs contain steps.
- Runners execute jobs.

---

# PART 3 – PRACTICAL LABS

## LAB 0 – Project Setup

### Objective

Create our initial project structure and connect it to GitHub.

### Step 1: Create the project folder

```bash
mkdir github-actions-playground

cd github-actions-playground
```

### Step 2: Create the folders

```bash
mkdir -p .github/workflows
mkdir -p src/css
mkdir -p src/js
mkdir -p tests
mkdir -p docker
```

### Step 3: Create the files

```bash
touch package.json
touch index.html
touch README.md
touch .gitignore
```

### Step 4: Initialize Git

```bash
git init

git branch -M main
```

### Step 5: Add and commit files

```bash
git add .

git commit -m "Initial project structure"
```

### Step 6: Create a GitHub repository

Repository name:

`github-actions-playground`

### Step 7: Connect local project to GitHub

```bash
git remote add origin https://github.com/YOUR-USERNAME/github-actions-playground.git
```

If the remote already exists, update it:

```bash
git remote set-url origin https://github.com/YOUR-USERNAME/github-actions-playground.git
```

Verify:

```bash
git remote -v
```

### Step 8: Push code

```bash
git push -u origin main
```

### Important Git Commands

| Command | Purpose |
|---|---|
| `git init` | Initialize Git repository |
| `git add .` | Stage changes |
| `git commit` | Save changes in Git history |
| `git push` | Upload commits to GitHub |
| `git status` | Check repository status |
| `git log --oneline` | View commit history |
| `git remote -v` | View remote repository |

**Lab 0 completed:** Initial project structure and GitHub connection.

---

## LAB 1 – GitHub Actions Fundamentals

### Objective

Understand the basic concepts before writing workflows.

Topics covered:

1. Events
2. Workflows
3. Jobs
4. Steps
5. Runners

### Knowledge Check

- `push` is an event.
- A workflow is a complete automation process.
- A job is a group of steps.
- A step is an individual task.
- A runner is the machine that executes jobs.

**Lab 1 completed:** Understood the five fundamental concepts.

---

## LAB 2 – First GitHub Actions Workflow

### Objective

Create a workflow that displays welcome messages whenever code is pushed to the main branch.

### Step 1: Create the workflow file

Location:

`.github/workflows/welcome.yaml`

### Step 2: Write the YAML code

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

### Step 3: Understand the configuration

| Configuration | Explanation |
|---|---|
| `name` | Workflow name |
| `on` | Defines the trigger |
| `push` | Trigger when code is pushed |
| `branches` | Specifies the branch |
| `jobs` | Defines jobs |
| `welcome` | Job name |
| `runs-on` | Selects the runner |
| `steps` | Defines individual tasks |
| `run` | Executes a shell command |

### Step 4: Push the workflow

```bash
git add .

git commit -m "Add first GitHub Actions workflow"

git push
```

### Step 5: Verify execution

1. Open your GitHub repository.
2. Navigate to the Actions tab.
3. Select My First GitHub Actions Workflow.
4. Open the latest workflow run.
5. Select the welcome job.
6. Expand both steps.

### Expected output

```text
Welcome to GitHub Actions!

My first automation is running successfully!
```

**Lab 2 completed:** Successfully created and executed the first GitHub Actions workflow.

---

## LAB 3 – Understanding GitHub-hosted Runners

### Objective

Understand how GitHub executes workflow jobs on its hosted machines.

### Step 1: Understand the runner configuration

```yaml
jobs:
  welcome:
    runs-on: ubuntu-latest
```

Explanation:

- `jobs` defines the jobs.
- `welcome` is the job name.
- `runs-on` selects the execution environment.
- `ubuntu-latest` selects a GitHub-hosted Ubuntu runner.

### Step 2: Different runner operating systems

| Configuration | Operating System |
|---|---|
| `ubuntu-latest` | Ubuntu Linux |
| `windows-latest` | Windows |
| `macos-latest` | macOS |

### Step 3: Add runner inspection commands

Add the following steps to `welcome.yaml`:

```yaml
      - name: Check runner operating system
        run: uname -a

      - name: Check current directory
        run: pwd

      - name: List project files
        run: ls -la
```

### Step 4: Push the changes

```bash
git add .

git commit -m "Explore GitHub Actions runner"

git push
```

### Step 5: Verify the output

Open:

GitHub Repository → Actions → Latest Workflow Run.

Observe:

- Linux operating system information.
- Current working directory.
- Files and folders visible in the runner workspace.

### Important Note: Checkout

At this stage, we have not added the checkout action.

Therefore, our repository source files have not yet been downloaded into the runner workspace.

We will learn `actions/checkout` in the next practical exercise.

**Lab 3 status:** Runner concepts introduced. Practical execution and checkout experiment to be completed.

---

# PART 4 – PROJECT FOLDER STRUCTURE

Our project will gradually grow into this structure:

```text
github-actions-playground/
│
├── .github/
│   └── workflows/
│       ├── welcome.yaml
│       ├── build.yaml
│       ├── test.yaml
│       ├── docker.yaml
│       └── deploy.yaml
│
├── src/
│   ├── css/
│   └── js/
│
├── tests/
│
├── docker/
│
├── index.html
├── package.json
├── README.md
└── .gitignore
```

---

# PART 5 – FUTURE LEARNING ROADMAP

| Lab | Topic |
|---|---|
| 4 | GitHub Actions triggers |
| 5 | Repository name, branch, commit ID and actor |
| 6 | Steps and shell commands |
| 7 | Multiple jobs and parallel execution |
| 8 | Sequential execution using `needs` |
| 9 | Build, test and deploy workflow |
| 10 | Job outputs |
| 11 | Environment variables |
| 12 | GitHub repository variables |
| 13 | GitHub Secrets |
| 14 | Docker Hub integration |
| 15 | Automatically build and push Docker images |
| 16 | Build our interactive webpage |
| 17 | Automated application testing |
| 18 | Security scanning |
| 19 | Docker image tagging and versioning |
| 20 | Automatic deployment |
| 21 | Reusable workflows |
| 22 | Artifacts and caching |
| 23 | Matrix strategies |
| 24 | Conditional execution |
| 25 | Scheduled workflows |
| 26 | Environments and approval gates |
| 27 | Deployment rollback |
| 28 | Workflow permissions and security |
| 29 | Troubleshooting |
| 30 | Complete end-to-end CI/CD project |

---

# FINAL PROJECT GOAL

By the end of this training, we aim to achieve the following automation:

1. Developer modifies the webpage.
2. Developer pushes code to GitHub.
3. GitHub Actions detects the push event.
4. Workflow checks out the source code.
5. Build job prepares the application.
6. Test job validates the application.
7. Security job scans for vulnerabilities.
8. Docker builds the application image.
9. Docker image is pushed to Docker Hub.
10. Application is deployed automatically.

The final project will demonstrate practical GitHub Actions, Docker and CI/CD skills.

---

**Learning Rule:** Never memorize a YAML file without understanding it. Every lab should include practical execution, verification and troubleshooting.

**Current Progress:** Lab 0 completed. Lab 1 completed. Lab 2 completed. Lab 3 in progress.

**Next Lab:** Lab 3 – Complete runner experiment and understand `actions/checkout`.