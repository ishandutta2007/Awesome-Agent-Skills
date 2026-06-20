# Using agent-skills with OpenClaw

OpenClaw can install local and GitHub-backed skill folders that contain a
`SKILL.md` file. Use this guide when you want the workflows in this repository
available through `openclaw skills`.

## Setup

### Option 1: Install Selected Skills

Clone the repository, then install the specific skill folders you want:

```bash
git clone https://github.com/addyosmani/agent-skills.git
openclaw skills install ./agent-skills/skills/test-driven-development
openclaw skills install ./agent-skills/skills/code-review-and-quality
```

OpenClaw derives each installed slug from the skill frontmatter `name`. Override
the slug only when you need a local alias:

```bash
openclaw skills install ./agent-skills/skills/security-and-hardening --as secure-review
```

### Option 2: Install for All Workspaces

Use `--global` for skills that should be available outside the current
workspace:

```bash
openclaw skills install ./agent-skills/skills/spec-driven-development --global
openclaw skills install ./agent-skills/skills/shipping-and-launch --global
```

### Option 3: Install from GitHub

OpenClaw supports GitHub skill installs when the repository root is a skill
folder. Because this repository stores multiple skills under `skills/`, clone it
first and install the individual folders locally.

## Verify Installation

List installed skills and inspect one entry:

```bash
openclaw skills list
openclaw skills info test-driven-development
openclaw skills check
```

If a skill does not appear, confirm the path points at a folder containing
`SKILL.md`.

## Recommended Skills

Start with a small set, then add more as the team adopts the workflows:

- `spec-driven-development` for shaping new work before implementation
- `test-driven-development` for behavior changes and bug fixes
- `code-review-and-quality` for structured review before merge
- `security-and-hardening` for user input, auth, secrets, and external services
- `shipping-and-launch` for release readiness

## Update Workflow

For local installs, pull the latest repository changes and reinstall the skill
folder:

```bash
cd agent-skills
git pull
openclaw skills install ./skills/test-driven-development --force
```

`openclaw skills update` is for ClawHub-tracked installs, so local folder
installs should use `--force`.
