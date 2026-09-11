## 1. The Core Distinction

GitHub supports two common ways to access repositories from the command line: **SSH** and **HTTPS**. The remote URL determines which protocol Git uses. The protocol and the credential are related, but they are not the same thing.

| Method | Remote URL                                | Main credential                                    | Typical test                |
| ------ | ----------------------------------------- | -------------------------------------------------- | --------------------------- |
| SSH    | `git@github.com:OWNER/REPOSITORY.git`     | SSH key pair                                       | `ssh -T git@github.com`     |
| HTTPS  | `https://github.com/OWNER/REPOSITORY.git` | Personal Access Token (PAT) or a credential helper | `git ls-remote <https-url>` |

The most important rule is:

> An SSH key is used with an SSH remote URL. A PAT or Git credential helper is used with an HTTPS remote URL.

GitHub no longer accepts an account password for Git operations over HTTPS. A PAT is entered in the password field when Git requests credentials; it is not the same as the GitHub account password.

## 2. SSH Authentication

SSH authentication uses a public/private key pair:

- The private key stays on the local computer.
- The public key is added to the GitHub account.
- GitHub verifies that the local computer holds the matching private key.

An SSH remote looks like this:

```text
git@github.com:OWNER/REPOSITORY.git
```

Generate a key, copy the public key, add it to GitHub, and test the connection:

```bash
ssh-keygen -t rsa -b 4096 -C "you@example.com"
pbcopy < ~/.ssh/id_rsa.pub
ssh -T git@github.com
```

The existing [[GitHub Repository Workflow]] note shows the complete SSH setup used in this knowledge base.

If a network blocks SSH port 22, SSH can still use port 443 through `ssh.github.com`. This is still **SSH authentication**, not HTTPS authentication:

```sshconfig
Host github.com
  Hostname ssh.github.com
  Port 443
  User git
```

## 3. HTTPS Authentication

An HTTPS remote looks like this:

```text
https://github.com/OWNER/REPOSITORY.git
```

There are two common ways to provide the HTTPS credential.

### 3.1 GitHub CLI and Browser Login

GitHub CLI can authenticate in a browser and store the resulting credential securely:

```bash
gh auth login --git-protocol https
gh auth status
gh auth setup-git
```

`gh auth setup-git` configures Git to use GitHub CLI as a credential helper. It does **not** change the repository's remote URL. The remote must still begin with `https://` for Git to use HTTPS.

### 3.2 Personal Access Token

When using Git directly, create a PAT and enter it when Git prompts for the HTTPS credentials:

```text
Username: YOUR-GITHUB-USERNAME
Password: YOUR-PERSONAL-ACCESS-TOKEN
```

On macOS, the Keychain helper can store the credential:

```bash
git config --global credential.helper osxkeychain
```

Do not place a PAT directly in a repository URL or commit it to a file.

## 4. Switching Between SSH and HTTPS

Check the current remote URL first:

```bash
git remote -v
```

Switch an existing remote to SSH:

```bash
git remote set-url origin git@github.com:OWNER/REPOSITORY.git
```

Switch an existing remote to HTTPS:

```bash
git remote set-url origin https://github.com/OWNER/REPOSITORY.git
```

After switching, use the credential method that matches the new URL. Changing the credential alone does not change the protocol.

## 5. Choosing a Method

SSH is usually convenient for a long-lived development machine because the key is used repeatedly without entering a PAT for every operation. HTTPS is often easier to use behind restrictive networks and integrates naturally with browser-based GitHub CLI login.

For the current repository notes:

- The Fork and repository-upload workflows use SSH URLs by default.
- Use HTTPS when SSH is blocked or when browser-based credential management is preferred.
- If SSH port 22 is blocked, configure SSH over port 443 before switching to HTTPS.

## 6. Common Errors

### 6.1 `Permission denied (publickey)`

Git is using an SSH remote, but the matching private key is unavailable, not loaded, or not associated with the GitHub account. Check the SSH key setup and run:

```bash
ssh -T git@github.com
```

### 6.2 `Authentication failed` or `Invalid username or token`

Git is probably using an HTTPS remote with an invalid, expired, or incorrectly stored credential. Check the remote URL, re-authenticate with `gh auth login --git-protocol https`, or replace the cached PAT.

### 6.3 The Remote and Credential Do Not Match

An SSH key cannot authenticate an HTTPS remote, and a PAT cannot authenticate an SSH remote. Check the URL with:

```bash
git remote -v
```

## 7. Related Notes and References

- [[GitHub CLI]]
- [[GitHub Fork Workflow]]
- [[GitHub Repository Workflow]]

Official references:

- [About authentication to GitHub](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/about-authentication-to-github)
- [Managing personal access tokens](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)
- [`gh auth login` manual](https://cli.github.com/manual/gh_auth_login)
- [`gh auth setup-git` manual](https://cli.github.com/manual/gh_auth_setup-git)
