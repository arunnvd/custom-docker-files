# Copilot Instructions for custom-docker-files

## Project Overview

This repository contains custom Docker images for development environments. Currently, it includes a single image: **ubuntu2404-dev-test-env**, which is an Ubuntu 24.04 LTS container configured for C/C++ development with Neovim and NvChad.

**Key purpose**: Provide a containerized, reproducible development environment for C/C++ code testing and practice with a modern terminal editor setup.

## Architecture

### Directory Structure
```
.
└── ubuntu2404-dev-test-env/
    └── Dockerfile          # Ubuntu 24.04 image with C/C++ tools and Neovim
```

### Dockerfile Design Patterns

1. **Multi-stage setup** (implicit):
   - Base image setup: Install system dependencies (build tools, git, curl, etc.)
   - Tool installation: Neovim, ripgrep (via upstream releases, not package manager)
   - User management: Create unprivileged `devuser` for running container
   - Editor configuration: Clone and configure NvChad at user level

2. **Key Environment Variables**:
   - `DEBIAN_FRONTEND=noninteractive` - Prevents interactive prompts during apt install
   - `USERNAME=devuser` (ARG) - Non-root user for container execution

3. **Shell Customization**:
   - `.bashrc` aliases created: `alias vim=nvim` and `alias grep=rg` (grep→ripgrep)
   - NvChad starter config cloned to `~/.config/nvim`

## Build and Test Commands

### Build the image
```bash
docker build -t ubuntu2404-dev-test-env ./ubuntu2404-dev-test-env
```

### Run a container (interactive shell)
```bash
docker run -it ubuntu2404-dev-test-env
```

### Run a container with volume mount (for local development)
```bash
docker run -it -v /path/to/local/code:/home/devuser/workspace ubuntu2404-dev-test-env
```

### Run a specific command in container
```bash
docker run ubuntu2404-dev-test-env cmake --version
docker run ubuntu2404-dev-test-env gcc --version
docker run ubuntu2404-dev-test-env nvim --version
```

### Validate image layers (no automated tests currently)
When modifying the Dockerfile, manually verify:
- `docker run ubuntu2404-dev-test-env nvim --version` - Neovim is installed
- `docker run ubuntu2404-dev-test-env rg --version` - ripgrep is installed
- `docker run ubuntu2404-dev-test-env gcc --version` - GCC is present
- `docker run ubuntu2404-dev-test-env bash -c 'alias'` - Shell aliases are configured

## Key Conventions

1. **Dockerfile layer optimization**:
   - Use `apt clean` and remove `/var/lib/apt/lists/*` after package installs to reduce layer size
   - Chain RUN commands with `&&` to reduce layer count
   - Install tools directly from upstream GitHub releases (Neovim, ripgrep) rather than package manager for specific versions

2. **User-level setup**:
   - User `root` used only for system-level changes (apt, sudo config)
   - Switch to unprivileged `devuser` (via `USER $USERNAME`) before editor/shell customization
   - All user config files created in `/home/$USERNAME` (NvChad, bash aliases)

3. **Non-root user privileges**:
   - `devuser` is added to `sudoers` with `NOPASSWD` for container convenience
   - User home is `/home/devuser` with shell `/bin/bash`

4. **Neovim/NvChad specifics**:
   - Neovim v0.11.6 installed from official GitHub release (not package manager)
   - NvChad starter config cloned to `~/.config/nvim`
   - Custom config space created at `~/.config/nvim/lua/custom` for user modifications

5. **Dependency versions**:
   - ripgrep: 15.1.0 (x86_64 musl binary)
   - Neovim: 0.11.6
   - Base: Ubuntu 24.04 LTS
   - Specify exact versions when updating upstream tools to ensure reproducibility

## Notes for Contributors

- When adding new tools or dependencies, prefer installing from source/GitHub releases if version pinning is important
- Keep the base OS minimal; avoid bloating with unnecessary packages
- Always test the container with `docker run` before committing Dockerfile changes
- Document any new environment variables or shell aliases in this file
