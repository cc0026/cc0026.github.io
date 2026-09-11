## 1. One-time Environment Setup

This workflow applies when you fork an open-source project, such as PyElastica or SophT, make local changes, push them to your own fork, and continue receiving updates from the original repository.

This note uses SSH remote URLs by default. For the distinction between SSH keys, HTTPS, PATs, and GitHub CLI credentials, see [[GitHub SSH vs HTTPS Authentication]].

### 1.1 SSH over Port 443

If a proxy such as Clash prevents ordinary SSH connections to GitHub, configure SSH to use GitHub's port 443 endpoint by adding the following block to `~/.ssh/config`:

```bash
cat >> ~/.ssh/config << 'EOF'

Host github.com
  Hostname ssh.github.com
  Port 443
  User git
EOF
```

Test the connection:

```bash
ssh -T git@github.com
```

A successful connection should return a message such as:

```text
Hi <username>! You've successfully authenticated...
```

### 1.2 HTTPS with a Personal Access Token

If SSH is not suitable, use an HTTPS remote with a **Personal Access Token (PAT)** or configure GitHub CLI as the credential helper. GitHub does not accept an account password for Git operations over HTTPS.

Create a token through **GitHub → Settings → Developer settings → Personal access tokens**. Prefer a fine-grained PAT with the minimum required repository permissions. Use a classic token with the `repo` scope only when the workflow requires it. Copy the token when it is created.

When Git asks for a password during `push`, paste the token. Nothing appears on screen while it is being pasted; this is normal.

When entering a PAT directly, store the credential in the macOS Keychain:

```bash
git config --global credential.helper osxkeychain
```

If you use GitHub CLI instead, run `gh auth login --git-protocol https` followed by `gh auth setup-git`. These commands manage HTTPS credentials but do not change the remote URL.

## 2. Fork the Repository on GitHub

1. Open the original repository, for example `https://github.com/GazzolaLab/PyElastica`.
2. Select **Fork** to create `<username>/PyElastica` under your account.
3. In the described workflow, a fork is public by default. If a private copy is required, change its visibility through **Settings → Danger Zone → Change visibility** after the fork is created.

## 3. Clone the Fork and Configure the Remotes

The two remotes have different roles:

- `origin` is your fork. Push your work here.
- `upstream` is the original author's repository. Fetch updates from here.

Clone your fork and add the upstream repository:

```bash
# Clone your own fork
git clone git@github.com:<username>/PyElastica.git
cd PyElastica

# Add the original repository for receiving updates
git remote add upstream git@github.com:GazzolaLab/PyElastica.git

# Verify both remotes
git remote -v
```

The commands below use SSH URLs. To use HTTPS instead, replace `git@github.com:OWNER/REPOSITORY.git` with `https://github.com/OWNER/REPOSITORY.git` and use the matching credential method.

The remote list should include entries similar to:

```text
origin    git@github.com:<username>/PyElastica.git
upstream  git@github.com:GazzolaLab/PyElastica.git
```

## 4. Exclude Large Files and Generated Output

Scientific-computing projects often generate large outputs such as `.pkl`, `.mp4`, and rendered frames. These files should not be uploaded to GitHub. GitHub also imposes a 100 MB limit on individual files.

Create or update `.gitignore` in the repository root before making local changes:

```bash
cat > .gitignore << 'EOF'
# ===== Computed output =====
*.pkl
*.mp4
*.avi
*.csv
*.npy
*.npz
*.h5
*.hdf5
*.dat
*.vtk
*.vtu

# Rendered animation frames
frames/
*.pov

# Output directories
results/
output/
data/

# Large installers, as needed
Miniconda3-latest-MacOSX-arm64.sh

# Python
__pycache__/
*.pyc
*.egg-info/
.pytest_cache/

# System, editor, and environment files
.DS_Store
.vscode/
.idea/
.venv/
venv/
EOF
```

`.gitignore` only applies to files that are not yet tracked. If a large file has already been committed, remove it from the index first:

```bash
git rm --cached <file>
```

## 5. Commit and Push Local Changes

### 5.1 Stage and Check Files

Stage the changes:

```bash
git add .
```

Before committing, list staged files larger than 1 MB:

```bash
git diff --cached --name-only | xargs -I{} find {} -size +1M 2>/dev/null
```

Ideally, the command produces no output. If it lists a file that should not be uploaded, add an appropriate rule to `.gitignore` and stage the changes again.

If the staged files are appropriate, commit and push them to your fork:

```bash
git commit -m "My local changes: briefly describe the changes"
git push origin main
```

### 5.2 If a Pre-commit Hook Blocks the Commit

Some projects run pre-commit checks that may fail because of configuration or version differences. If the immediate goal is to create a code backup, the commit can be made without running the hook:

```bash
git commit --no-verify -m "My local changes: briefly describe the changes"
```

### 5.3 If Push Is Rejected

If `push` reports `rejected` or `non-fast-forward`, the remote contains commits that are not present locally. Do not use `--force` immediately. Fetch and merge the remote changes first:

```bash
git fetch origin
git merge origin/main      # or origin/master, depending on the remote branch
# Resolve conflicts if necessary, then push again
```

## 6. Synchronize Updates from the Upstream Repository

When the original author publishes new changes, fetch and merge them into your current branch:

```bash
# 1. Download the upstream history without changing the working tree
git fetch upstream

# 2. Merge the upstream branch into the current branch
git merge upstream/<upstream-default-branch>

# 3. Push the merged result to your own fork
git push origin main
```

The upstream default branch varies by project. Use the correct branch name:

- PyElastica uses `master`: `git merge upstream/master`
- SophT uses `main`: `git merge upstream/main`

## 7. Resolve Merge Conflicts

If the upstream author changed the same file that you modified, Git may report a merge conflict. The file will contain markers similar to:

```text
<<<<<<< HEAD
Your content
=======
Upstream content
>>>>>>> upstream/master
```

Edit the file to keep the intended content, then remove the conflict markers. Complete the merge and push the result:

```bash
git add <conflicted-file>
git commit            # Complete the merge commit
git push origin main
```

## 8. Project-specific Branch Reference

| Project    | Branch in your fork | Upstream branch | Synchronization command                           |
| ---------- | ------------------- | --------------- | ------------------------------------------------- |
| PyElastica | `main`              | `master`        | `git fetch upstream && git merge upstream/master` |
| SophT      | `main`              | `main`          | `git fetch upstream && git merge upstream/main`   |

The fork-side branch is `main` in this workflow. The upstream branch name is controlled by the original repository and may differ between projects, so the synchronization command must be adapted accordingly.

## 9. Related Notes

- [[GitHub CLI]]
- [[GitHub SSH vs HTTPS Authentication]]
