# nixomatic-skill

An agent skill that gives you and your agents instant access to **any software**
on-the-fly using [nixomatic](https://nixomatic.com).

Compatible with any agent that supports the skills convention, including
[Claude Code](https://docs.anthropic.com/en/docs/claude-code) and
[Codex](https://openai.com/index/introducing-codex/).

Need to convert a PDF to text? Resize an image? Transcode video? Build a Rust
project? This skill lets your agent run any tool available in
[nixpkgs](https://search.nixos.org/packages) — instantly, in one shot, without
altering the current system. It works for one-off tasks (e.g. `pdftotext` via
`poppler-utils`, `ffmpeg`, `imagemagick`) as well as full development
environments detected from project files (`package.json`, `Cargo.toml`,
`go.mod`, etc.).

For project development environments, it also keeps a
`## Development Environment` section in the project README so anyone can
reproduce the environment.

Works with **Nix** directly or via **Docker** (`nixos/nix`).

## Install

### Using [skills.sh](https://skills.sh)

```bash
# Per-project
npx skills add -s '*' -a '*' curriedsoftware/nixomatic-skill
```

```bash
# Global (all projects)
npx skills add -s '*' -a '*' -g curriedsoftware/nixomatic-skill
```

### Manual

#### Per-project

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

#### Global (all projects)

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

**For one-shot tasks** (file conversion, data processing, any ad-hoc tool use):

1. Identifies the nixpkgs package that provides the tool you need.
2. Runs the command inside `nix develop` with a nixomatic URL — instantly,
   without installing anything permanently.

**For project development environments:**

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
