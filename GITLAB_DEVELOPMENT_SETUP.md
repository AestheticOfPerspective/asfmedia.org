# GitLab Development Setup

## Why GitLab Matters Here

`asfmedia.org` is at the point where local files are no longer enough.

We now want:

- one real remote source of truth
- clean version history
- a simple deployment handoff
- a place to track the development Lernreise

This does not need a heavy platform setup on day one. It needs a clean account, SSH access, one repo, and one predictable workflow.

## Recommended First Goal

Set up one GitLab account and one repository for `asfmedia.org`, then use that repo as the deployment source for the static site relaunch.

## Two Possible GitLab Targets

### Option A: Existing self-hosted GitLab

Memory context suggests there may already be a GitLab instance at:

- `git.labs.ooo`
- SSH via port `30022`

Use this if:

- you already control that environment
- you want the Lernreise inside your own ecosystem
- SSH access and account provisioning are available

### Option B: GitLab.com

Use this if:

- you want the fastest setup
- the self-hosted instance is not ready or not accessible right now
- you need a stable external remote immediately

## Recommended Naming

### Account / namespace

Good simple options:

- `asfmedia`
- `alexandersolod`
- `asf-media`

### Project name

- `asfmedia.org`

Keep it boring and obvious.

## Minimal Setup Checklist

### 1. Create or confirm the GitLab account

Need:

- username
- email
- 2FA if possible
- SSH key support

### 2. Create an SSH key if needed

Check first:

```bash
ls ~/.ssh
```

If needed, create one:

```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

### 3. Add the public key to GitLab

Show the public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

Paste it into GitLab SSH settings.

### 4. Create the repo

Create:

- namespace: your account or group
- repo: `asfmedia.org`
- visibility: private first, unless there is a reason to go public immediately

### 5. Connect the local repo

For GitLab.com:

```bash
git remote add origin git@gitlab.com:<namespace>/asfmedia.org.git
```

For the self-hosted GitLab pattern from memory:

```bash
git remote add origin ssh://git@git.labs.ooo:30022/<namespace>/asfmedia.org.git
```

### 6. Make the first commit

Suggested first commit scope:

- `index.html`
- `README.md`
- `traffic-plan.md`
- `ALFAHOSTING_DEPLOY_CHECKLIST.md`
- this GitLab setup file

### 7. Push the repo

```bash
git add .
git commit -m "Initialize ASF Media static site"
git push -u origin master
```

If you prefer `main`, rename before pushing.

## Branch Strategy For The Lernreise

Keep this extremely light at first.

### Suggested setup

- `main` or `master` = deployable branch
- short feature branches only when needed

Examples:

- `feature/landing-polish`
- `feature/contact-flow`
- `feature/portfolio-structure`

## Use GitLab For More Than Storage

GitLab should hold the development Lernreise too.

Good first artifacts:

- issues for domain, DNS, billing, launch
- milestones for relaunch phases
- simple README-based changelog

### Suggested first issues

1. Fix Alfahosting billing and confirm account status
2. Snapshot current DNS zone
3. Create GitLab remote and first push
4. Decide whether `web.asfmedia.org` or root goes first
5. Connect deploy target to git repo

## Lowest-Risk Workflow

1. local edits
2. commit to GitLab
3. deploy preview or static host
4. only then touch DNS

This keeps hosting mistakes separate from content/design work.

## What Not To Overcomplicate Yet

- no CI pipeline required on day one
- no Docker needed
- no monorepo needed
- no giant branching model needed

First win is simply:

`local repo -> GitLab repo -> deploy target -> DNS cutover`

## Best Next Move

Right now the strongest next step is:

1. decide `GitLab.com` vs `git.labs.ooo`
2. create or confirm the account
3. add SSH key
4. create `asfmedia.org` repo
5. connect local repo remote

Once that is done, the web relaunch stops being loose local work and becomes a real development track.
