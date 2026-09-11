## 1. Overview

GitHub CLI (`gh`) is the official command-line interface for GitHub. It allows GitHub authentication and many common GitHub operations to be performed directly from the terminal.

Official website: https://cli.github.com/

For the difference between SSH and HTTPS repository authentication, see [[GitHub SSH vs HTTPS Authentication]].

## 2. Installation

On macOS, GitHub CLI can be installed using Homebrew:

```bash
brew install gh
```

Check whether it is installed correctly:

```bash
gh --version
```

## 3. Authentication

GitHub no longer supports password authentication for Git operations over HTTPS. GitHub CLI provides a convenient way to authenticate Git and other GitHub operations.

Start the interactive authentication flow with:

```bash
gh auth login
```

For a normal GitHub account, select:

```text
GitHub.com
HTTPS
Login with a web browser
```

The protocol can also be selected explicitly:

```bash
gh auth login --git-protocol https
gh auth login --git-protocol ssh
```

GitHub CLI will display a one-time code in the terminal:

```text
First copy your one-time code: XXXX-XXXX
```

The code is **not sent by email or SMS**.

Copy this code and enter it on the GitHub device authorization page opened in the browser.

In the first setup, **HTTPS** is the Git protocol selected for repository operations. The browser is only used to authorize GitHub CLI; this does not mean that Git uses the GitHub account password.

After authorization, check the authentication status:

```bash
gh auth status
```

A successful login should show that the current GitHub account is authenticated.

## 4. Git Authentication

If the repository uses an HTTPS remote, GitHub CLI can configure Git to use the GitHub CLI credential helper:

```bash
gh auth setup-git
```

This configures credential handling; it does not change an existing remote URL from HTTPS to SSH or from SSH to HTTPS.

After authentication, normal Git commands can be used:

```bash
git clone ...
git pull
git push
```

GitHub CLI handles the authentication in the background.

## 5. Common Commands

### 5.1 Authentication

Check the current authentication status:

```bash
gh auth status
```

Log in:

```bash
gh auth login
```

Log out:

```bash
gh auth logout
```

Switch between authenticated accounts:

```bash
gh auth switch
```

### 5.2 Repository

Create a repository:

```bash
gh repo create
```

Clone a repository:

```bash
gh repo clone OWNER/REPOSITORY
```

View the current repository:

```bash
gh repo view
```

Open the current repository in a browser:

```bash
gh repo view --web
```

### 5.3 Pull Requests

List pull requests:

```bash
gh pr list
```

Create a pull request:

```bash
gh pr create
```

View a pull request:

```bash
gh pr view
```

### 5.4 Issues

List issues:

```bash
gh issue list
```

Create an issue:

```bash
gh issue create
```

### 5.5 GitHub Actions

List recent workflow runs:

```bash
gh run list
```

View a workflow run:

```bash
gh run view
```

## 6. `git` vs `gh`

`git` and `gh` serve different purposes.

```text
git
│
├── commit
├── branch
├── merge
├── push
└── pull

gh
│
├── GitHub authentication
├── repositories
├── pull requests
├── issues
├── Actions
└── releases
```

`git` is the version-control system, while `gh` is an interface for interacting with GitHub.

They are commonly used together.

## 7. Related Notes

- [[Quartz Installation and Deployment]]
- [[GitHub SSH vs HTTPS Authentication]]
