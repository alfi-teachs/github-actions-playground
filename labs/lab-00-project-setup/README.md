# Lab 0 – GitHub Actions Project Setup

## Objective
Create a project structure for learning GitHub Actions and prepare it for version control using Git and GitHub.

## Prerequisites
- Git
- Visual Studio Code
- GitHub account
- Git Bash

## Project Structure

```text
github-actions-playground/
├── .github/
│   └── workflows/
├── labs/
│   ├── lab-00-project-setup/
│   │   └── README.md
│   ├── lab-01-fundamentals/
│   │   └── README.md
│   ├── lab-02-first-workflow/
│   │   └── README.md
│   ├── lab-03-runners/
│   │   └── README.md
│   └── lab-04-triggers/
│       └── README.md
├── src/
│   ├── css/
│   └── js/
├── tests/
├── docker/
├── index.html
├── package.json
├── README.md
└── .gitignore
```

## Important Git Commands

| Command | Purpose |
|---|---|
| `git init` | Initialize a Git repository |
| `git status` | Check working-tree status |
| `git add .` | Stage changes |
| `git commit -m "message"` | Save staged changes |
| `git push` | Upload commits to GitHub |
| `git pull` | Download and integrate remote changes |
| `git remote -v` | View configured remote repositories |
| `git log --oneline` | View commit history |

## What I Learned

- How to create a project directory and organize files.
- How to initialize a Git repository.
- How to connect a local repository to GitHub.
- How to commit and push project changes.
- Why a clean folder structure matters for automation projects.

## Verification

The project is connected to GitHub, and the local branch is `main`.

## Outcome

The project is prepared for hands-on GitHub Actions labs.