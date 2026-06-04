# langzi-skills

Personal Claude Code skills repository.

This repository uses a source-library layout for reusable skills. The canonical skill source lives under `skills/<skill-name>/`.

## Layout

```text
langzi-skills/
├── skills/
│   └── langzi-project-memory/
│       ├── SKILL.md
│       └── references/
│           ├── start.md
│           ├── propose.md
│           └── finish.md
└── README.md
```

## Skills

### `langzi-project-memory`

Manages project-level long-term memory files under a project's `memory/` directory.

Usage in Claude Code:

```text
/langzi-project-memory start
/langzi-project-memory propose
/langzi-project-memory finish
```

## Source vs installed copy

- `skills/` is the repository source of truth.
- `.claude/skills/` is treated as local Claude Code installed state and is ignored by Git.
- Do not edit an installed copy directly unless you also sync the change back to `skills/`.

## Install for Claude Code / cc-switch

Claude Code user-level skills are usually discovered from:

```text
C:\Users\Administrator\.claude\skills\
```

To expose this skill globally, either copy it:

```powershell
Copy-Item -Recurse -Force \
  "D:\PythonProject\langzi-skills\skills\langzi-project-memory" \
  "$env:USERPROFILE\.claude\skills\langzi-project-memory"
```

Or link it so this repository remains the single source of truth:

```powershell
New-Item -ItemType Junction \
  -Path "$env:USERPROFILE\.claude\skills\langzi-project-memory" \
  -Target "D:\PythonProject\langzi-skills\skills\langzi-project-memory"
```

After copying or linking, refresh or rescan skills in cc-switch if needed.

## Contract boundary

This repository contains Claude Code skill source files. Repository layout changes here do not define or change application APIs, database schemas, storage contracts, permissions, state machines, or error codes.
