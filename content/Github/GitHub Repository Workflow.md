## 1. Create a Repository on GitHub

Create the repository through the GitHub web interface. The following screenshot records the repository entry point in the account interface:

![[GitHub Repository Workflow/repository-menu.png]]

## 2. Install Git

On macOS, install Git with Homebrew:

```bash
brew install git
```

## 3. Configure Git and SSH

This note uses SSH URLs for `clone`, `push`, and `pull`. For the HTTPS alternative, see [[GitHub SSH vs HTTPS Authentication]].

Configure the global Git identity, generate an SSH key, and copy the public key so it can be added to the SSH key list on GitHub:

```bash
# Configure the global Git identity
git config --global user.name "cc0026"
git config --global user.email "zhuoling_ran@163.com"

# Generate an SSH key
ssh-keygen -t rsa -b 4096 -C "zhuoling_ran@163.com"
pbcopy < ~/.ssh/id_rsa.pub
```

Add the copied public key to GitHub, then test the connection:

![[GitHub Repository Workflow/ssh-keys.png]]

![[GitHub Repository Workflow/ssh-key-generation.png]]

```bash
ssh -T git@github.com
```

## 4. Upload Code to a Repository

Initialize the local repository, create the first commit, connect it to the GitHub repository, and push the `main` branch:

```bash
# Initialize the local repository and create the first commit
git init
git add .
git commit -m "Describe the initial changes"

# Configure the GitHub remote
git remote rm origin
git remote add origin git@github.com:cc0026/Jellyfish.git

# Push the main branch
git branch -M main
git push -u origin main
```

If the local repository does not already have an `origin` remote, omit `git remote rm origin`.

## 5. Update the Repository from Another Computer

### 5.1 Clone the Repository

On a second computer, clone the repository and enter its directory:

```bash
git clone git@github.com:cc0026/Jellyfish.git
cd Jellyfish
```

### 5.2 Pull, Commit, and Push Changes

Pull the latest remote state before editing, then commit and push local changes:

```bash
git pull origin main

git add .
git commit -m "Update from the second computer"
git push origin main
```

## 6. Related Notes

- [[GitHub CLI]]
- [[GitHub Fork Workflow]]
- [[GitHub SSH vs HTTPS Authentication]]
