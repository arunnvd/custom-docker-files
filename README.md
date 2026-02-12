# Custom Docker Files & Containerization Reference

A personal repository for custom Docker images, docker-compose configurations, and documentation of my learning journey with Docker and containerization.

**Note**: While this repository is public, it primarily serves as a personal reference and learning resource. The images and configurations here are tailored to my specific workflows and may require adaptation for other use cases.

## Repository Structure

```
custom-docker-files/
├── ubuntu2404-dev-test-env/     # Ubuntu 24.04 development environment
│   └── Dockerfile               # Configured for C/C++ development with Neovim
├── .github/
│   └── copilot-instructions.md  # Context for AI-assisted development
└── README.md                     # This file
```

## Current Images

### ubuntu2404-dev-test-env

**Purpose**: A containerized development environment for C/C++ code testing, practice, and development.

**What's Included**:
- Base: Ubuntu 24.04 LTS
- C/C++ toolchain: GCC, Clang, CMake, build-essential
- Editor: Neovim v0.11.6 with NvChad configuration
- Utilities: ripgrep (for fast code searching), Git, curl
- Unprivileged user (`devuser`) with sudo access

**Quick Start**:
```bash
# Build the image
docker build -t ubuntu2404-dev-test-env ./ubuntu2404-dev-test-env

# Run interactively
docker run -it ubuntu2404-dev-test-env

# Mount local code for development
docker run -it -v $(pwd):/home/devuser/workspace ubuntu2404-dev-test-env
```

**Key Features**:
- Modern terminal editor setup (Vim alias → Neovim, grep alias → ripgrep)
- NvChad starter config with customization space
- Fully configured shell environment
- Reproducible development setup

For more detailed information, see [`.github/copilot-instructions.md`](.github/copilot-instructions.md).

## What You'll Find Here

This repository documents:

✅ **Custom Dockerfile configurations** - Development environments optimized for specific workflows  
✅ **Docker best practices** - Layer optimization, non-root user setup, versioning strategies  
✅ **Containerization patterns** - Multi-stage builds, volume management, networking (as they're added)  
✅ **Learning notes** - Commentary on design decisions and containerization concepts  

## Future Additions

This repository will grow to include:
- Additional Docker images (Python, Node.js, full-stack dev environments, etc.)
- Docker Compose files for multi-container setups
- Documentation on networking, persistence, and production patterns
- Comparison notes on different containerization approaches

## Usage Notes

### For Your Own Use
- Clone or pull the latest image configurations
- Build and customize as needed for your workflows
- Mount local volumes to test code changes
- Modify Dockerfiles in subdirectories to suit your environment

### For Others Exploring This Repository
- These images are optimized for my personal workflow
- You may need to adjust tool versions, dependencies, or configurations
- Use this as a reference for Docker patterns and best practices
- Feel free to fork and adapt for your own needs

## Prerequisites

- Docker Engine installed ([Install Docker](https://docs.docker.com/get-docker/))
- Sufficient disk space for image builds
- Basic familiarity with Docker commands

## Common Commands

```bash
# Build an image
docker build -t image-name ./path-to-dockerfile

# List all images
docker images

# Run a container
docker run -it image-name

# Remove an image
docker rmi image-name

# View image layers
docker history image-name
```

## Learning Resources

Some concepts documented in this repo are informed by:
- Official Docker documentation
- Personal experimentation and iteration
- Container best practices (security, efficiency, reproducibility)

## License

Personal reference repository. Feel free to use, modify, and learn from these configurations.

## Questions or Suggestions?

While this is primarily a personal reference, constructive feedback is welcome. For issues specific to your environment, consider adapting the configurations to your needs.

---

**Last Updated**: February 2026

**Maintainer**: Arun Raj

**Note**: README Generated using AI
