# Lab 1 – GitHub Actions Fundamentals

## Objective
Understand the basic components of GitHub Actions and how they work together to automate tasks.

## 1. What is GitHub Actions?

GitHub Actions is an automation platform built into GitHub. It allows us to automatically run tasks when events happen in a repository.

Examples:
- Run tests when code is pushed.
- Build an application.
- Scan code for security issues.
- Deploy an application.

## 2. Important Components

| Component | Meaning |
|---|---|
| Workflow | An automated process defined in a YAML file |
| Event | An activity that triggers a workflow |
| Job | A group of steps executed on a runner |
| Step | An individual task inside a job |
| Runner | The machine or environment that executes a job |
| Action | A reusable unit of automation |

## 3. How They Work Together

1. An event occurs, such as a push to GitHub.
2. The event triggers a workflow.
3. The workflow starts one or more jobs.
4. A runner executes each job.
5. Each job performs its steps.

## 4. Workflow File Location

GitHub Actions workflow files must be stored in:

```text
.github/workflows/
```

Workflow files use YAML extensions such as `.yml` or `.yaml`.

## 5. Types of Runners

| Runner | Explanation |
|---|---|
| GitHub-hosted runner | A machine managed by GitHub |
| Self-hosted runner | A machine that we configure and manage ourselves |

Example:

```yaml
runs-on: ubuntu-latest
```

This tells GitHub Actions to execute the job on a GitHub-hosted Ubuntu runner.

## 6. Important Concepts

- Steps inside a job run sequentially by default.
- Jobs run in parallel by default.
- We can use the `needs` keyword to make one job wait for another.
- A runner executes the job independently of our local computer.

## 7. What I Learned

- What GitHub Actions is.
- The difference between workflows, jobs, steps, and runners.
- How events trigger workflows.
- The difference between GitHub-hosted and self-hosted runners.
- Where workflow YAML files are stored.

## Outcome

I understand the basic structure of GitHub Actions and how its components work together.