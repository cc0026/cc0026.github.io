Quartz is a static site generator designed for publishing Markdown-based notes and digital gardens.

It can convert local Markdown notes into a website with features such as WikiLinks, backlinks, search, tags, and graph view.

Official website: https://quartz.jzhao.xyz/

## Workflow

The workflow used here is:

```text
Markdown Files
  ↓
Quartz
  ↓
Local Preview
  ↓
GitHub
  ↓
GitHub Actions
  ↓
GitHub Pages
  ↓
Public Blog
```

## Requirements

Quartz requires:

- Node.js
- npm
- Git

Check whether they are installed:

```bash
node -v
npm -v
git --version
```

On macOS, they can be installed using Homebrew:

```bash
brew install node
brew install git
```

## Install Quartz

Go to the directory where the website will be stored:

```bash
cd ~/Documents
```

Clone Quartz:

```bash
git clone https://github.com/jackyzha0/quartz.git
```

Enter the directory:

```bash
cd quartz
```

Install dependencies:

```bash
npm install
```

Initialize Quartz:

```bash
npx quartz create
```

The resulting directory contains files such as:

```text
quartz/
├── content/
├── quartz/
├── quartz.config.ts
├── quartz.layout.ts
└── package.json
```

The most important directory for normal use is:

```text
content/
```

Markdown files placed here become pages on the website.

## Create the Homepage

Create:

```text
content/index.md
```

For example:

```markdown
# My Notes

Welcome to my personal knowledge blog.

## Topics

- [[Materials Science]]
- [[Machine Learning]]
- [[GitHub]]
```

`index.md` is the homepage of the Quartz website.

If it is missing or unavailable, opening the root URL may result in a `404` page.

## Local Preview

From the Quartz directory:

```bash
npx quartz build --serve
```

Quartz will normally serve the website at:

```text
http://localhost:8080
```

This is only a **local preview**.

```text
localhost
     ↓
only accessible from the local computer
```

It is not yet a publicly deployed website.

## Add Markdown Notes

Quartz uses Markdown files as its content input.

Place Markdown files inside the `content/` directory. For example:

```text
Quartz
└── content/
    └── Materials Science/
        └── Transformation Stress.md
```

A useful structure might be:

```text
content/
├── index.md
├── Materials Science/
├── Machine Learning/
├── GitHub/
└── Paper Notes/
```

Markdown files can be created or edited with any Markdown-compatible editor. Once placed inside `content/`, Quartz can convert them into website pages.

## Create a GitHub Repository

To publish the website, create a repository on GitHub.

For a personal website, the repository can be named:

```text
USERNAME.github.io
```

This gives a public URL such as:

```text
https://USERNAME.github.io/
```

Alternatively, a repository can have a project name such as:

```text
notes
```

which normally produces:

```text
https://USERNAME.github.io/notes/
```

## Connect Quartz to the GitHub Repository

Because Quartz was cloned from the official Quartz repository, first check the existing remote:

```bash
git remote -v
```

It may still point to:

```text
https://github.com/jackyzha0/quartz.git
```

Remove the existing `origin`:

```bash
git remote remove origin
```

Add the personal repository:

```bash
git remote add origin https://github.com/USERNAME/USERNAME.github.io.git
```

Check again:

```bash
git remote -v
```

## GitHub Authentication

GitHub does not support account-password authentication for Git operations over HTTPS.

GitHub CLI can be used instead.

See [[GitHub CLI]].

Install it with:

```bash
brew install gh
```

Authenticate:

```bash
gh auth login
```

A common configuration is:

```text
GitHub.com
HTTPS
Login with a web browser
```

Check authentication:

```bash
gh auth status
```

If necessary, configure Git to use GitHub CLI credentials:

```bash
gh auth setup-git
```

## Push Quartz to GitHub

Stage the files:

```bash
git add .
```

Create a commit:

```bash
git commit -m "Initial Quartz blog"
```

Set the main branch:

```bash
git branch -M main
```

Push:

```bash
git push -u origin main
```

At this point, the Quartz source files are stored on GitHub.

However, uploading the repository is **not the same as deploying the website**.

## Configure Quartz

Website settings are primarily stored in:

```text
quartz.config.ts
```

For example, change the website title:

```ts
pageTitle: "My Notes",
```

For a personal GitHub Pages website:

```text
https://USERNAME.github.io/
```

set:

```ts
baseUrl: "USERNAME.github.io",
```

The protocol `https://` should not be included in `baseUrl`.

## Deploy with GitHub Pages

GitHub Pages hosts the generated static website.

The deployment pipeline is:

```text
Markdown
   ↓
Quartz
   ↓
GitHub Actions
   ↓
Build website
   ↓
GitHub Pages
```

In the GitHub repository, go to:

```text
Settings
→ Pages
```

Under **Build and deployment**, select:

```text
Source
→ GitHub Actions
```

Quartz uses a GitHub Actions workflow to build and deploy the website.

Because the recommended workflow can change between Quartz versions, use the current Quartz documentation when configuring it:

https://quartz.jzhao.xyz/hosting

## Check Deployment

Open the repository's:

```text
Actions
```

section.

A successful deployment should show the build/deployment workflow completing successfully.

For example:

```text
Build   ✓
Deploy  ✓
```

The website can then be accessed at:

```text
https://USERNAME.github.io/
```

Unlike `localhost`, this address can be shared with other people.

## Local Preview vs Public Deployment

| | Local Preview | GitHub Pages |
|---|---|---|
| Address | `localhost:8080` | `USERNAME.github.io` |
| Accessible by others | No | Yes |
| Requires local server | Yes | No |
| Available when computer is off | No | Yes |
| Purpose | Testing | Publication |

## Updating the Blog

After the initial deployment, adding a new note is much simpler.

Create or copy a Markdown file into:

```text
content/
```

Preview locally:

```bash
npx quartz build --serve
```

Then publish the changes:

```bash
git add .
git commit -m "Update notes"
git push
```

GitHub Actions will rebuild and redeploy the website automatically.

Therefore, the normal publishing workflow becomes:

```text
Markdown File
  ↓
content/
  ↓
Local Preview
  ↓
git add
  ↓
git commit
  ↓
git push
  ↓
GitHub Actions
  ↓
Public Blog Updated
```

## Common Commands

Start local preview:

```bash
npx quartz build --serve
```

Check repository status:

```bash
git status
```

Check remote repository:

```bash
git remote -v
```

Publish changes:

```bash
git add .
git commit -m "Update notes"
git push
```

Check GitHub authentication:

```bash
gh auth status
```

## Related Notes

- [[GitHub CLI]]
- [[Git]]
- [[GitHub]]
- [[GitHub Pages]]
- [[GitHub Actions]]
- [[Markdown]]