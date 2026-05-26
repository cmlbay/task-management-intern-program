# Contributing Guide

This guide explains the workflow you must follow throughout the internship program.
Please read it carefully before opening your first Pull Request.

## Getting Started

### 1. Fork the Repository
- Go to the mentor's repository
- Click the `Fork` button in the top right corner
- Clone your fork to your local machine

```bash
git clone https://github.com/YOUR_USERNAME/task-management-intern-program.git
cd task-management-intern-program
```

### 2. Set Up Upstream Remote
This allows you to pull updates from the mentor's repository:

```bash
git remote add upstream https://github.com/MENTOR_USERNAME/task-management-intern-program.git
```

### 3. Create Your Working Branch
Never work directly on `master`. Always create a feature branch:

```bash
git checkout dev
git pull upstream dev
git checkout -b feature/week-1-setup
```

## Branch Naming Convention

| Branch | Pattern | Example |
|---|---|---|
| Weekly work | `feature/week-{n}-{topic}` | `feature/week-1-setup` |
| Bug fix | `fix/{topic}` | `fix/jwt-token-expiry` |
| Documentation | `docs/{topic}` | `docs/update-readme` |

## Commit Message Convention
Follow this format for all commit messages:
| Type | When to use |
|---|---|
| `feat` | Adding a new feature |
| `fix` | Fixing a bug |
| `docs` | Documentation changes |
| `refactor` | Code refactoring |
| `test` | Adding tests |

**Examples:**
- `#feat | auth > add JWT token generation`
- `#fix | task > correct status flow validation`
- `#docs | week-1 > update setup instructions`
- `#refactor | project > extract authorization to service layer`
- `#test | task > add unit tests for TaskService`
- `#chore | config > update application.yml structure`

**Rules:**
- Use lowercase
- Use present tense ("add" not "added")
- Keep it under 72 characters
- No period at the end

## Pull Request Process

### When to Open a PR
Open a Pull Request at the end of each week when your deliverables are complete.

### Steps
1. Push your branch to your fork:
```bash
git push origin feature/week-1-setup
```

2. Go to your fork on GitHub
3. Click `Compare & pull request`
4. Set the base repository to the mentor's repo, base branch to `master`
5. Fill out the PR template completely
6. Submit the PR

### PR Title Format
[Week N] Topic Description
Examples:
- `[Week 1] Setup & Git Flow`
- `[Week 2] Entities & Authentication`
- `[Week 3] Project & Member Management`
- `[Week 4] Task Management & Final Delivery`

## Code Review Process

- Mentor will review your PR and leave comments
- Address all comments before requesting re-review
- Do not merge your own PR without mentor approval
- Once approved, mentor will merge the PR

## Important Rules

- Never commit passwords, secrets, or API keys
- Always add `application.yml` to `.gitignore`
- Do not push directly to `master` or `dev`
- One PR per week — do not mix multiple weeks in one PR
- Keep your fork up to date with upstream before starting each week

## Need Help?

- If you are stuck for more than 30 minutes, ask for help
- Describe what you tried before asking
- Share relevant code snippets or error messages
