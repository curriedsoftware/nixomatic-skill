# nixomatic-skill

An agent skill that sets up reproducible development environments using
[nixomatic](https://nixomatic.com).

Compatible with any agent that supports the skills convention, including
[Claude Code](https://docs.anthropic.com/en/docs/claude-code) and
[Codex](https://openai.com/index/introducing-codex/).

When you ask your agent to build, test, lint, format, or set up a project, this
skill automatically detects the required packages from project files (e.g.
`package.json`, `Cargo.toml`, `go.mod`) and runs commands inside a Nix
environment — no `flake.nix` authoring required. It also keeps a
`## Development Environment` section in the project README so anyone can
reproduce the environment.

Works with **Nix** directly or via **Docker** (`nixos/nix`).

## Install

### Per-project

Symlink or copy the `nixomatic` directory into your project's skills directory:

```bash
# From your project root (Claude Code)
mkdir -p .claude/skills
ln -s /path/to/nixomatic-skill/nixomatic .claude/skills/nixomatic
```

```bash
# From your project root (Codex)
mkdir -p .codex/skills
ln -s /path/to/nixomatic-skill/nixomatic .codex/skills/nixomatic
```

### Global (all projects)

Place it under the global skills directory so every project picks it up:

```bash
# Claude Code
mkdir -p ~/.claude/skills
ln -s /path/to/nixomatic-skill/nixomatic ~/.claude/skills/nixomatic
```

```bash
# Codex
mkdir -p ~/.codex/skills
ln -s /path/to/nixomatic-skill/nixomatic ~/.codex/skills/nixomatic
```

Replace `/path/to/nixomatic-skill` with the actual path where you cloned this
repository.

## Prerequisites

You need at least one of:

- [Nix](https://nixos.org/download/) (with flakes support)
- [Docker](https://docs.docker.com/get-docker/) (uses the `nixos/nix` image)

## How it works

1. Scans project files to detect languages, runtimes, and build tools.
2. Constructs a `https://nixomatic.com/?p=pkg1,pkg2,...` URL with the required
   Nix packages.
3. Runs commands inside `nix develop` (or the Docker equivalent).
4. If a tool is missing at runtime, adds the package and retries.
5. Updates the project README with the working nixomatic URL so the environment
   is reproducible.

See [`nixomatic/SKILL.md`](nixomatic/SKILL.md) for the full skill definition
including package mappings and command templates.

## License

MIT
