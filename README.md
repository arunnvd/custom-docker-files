# Custom Docker Files & Containerization Reference

A personal repository for custom Docker images, docker-compose configurations, and documentation of my learning journey with Docker and containerization.

**Note**: While this repository is public, it primarily serves as a personal reference and learning resource. The images and configurations here are tailored to my specific workflows and may require adaptation for other use cases.

## Repository Structure

```
custom-docker-files/
├── ubuntu2404-dev-test-env/     # Ubuntu 24.04 development environment
│   └── Dockerfile               # Configured for C/C++ development with Neovim
├── react-android-linux/         # React Native Android build environment
│   └── Dockerfile               # Configured for React Native & Android builds
├── vlc-build-env/               # VLC source code compilation environment
│   └── Dockerfile               # Ubuntu 22.04 with VLC build dependencies
├── vlc-libvlc-jni/              # libvlc-jni Android bindings build environment
│   └── Dockerfile               # Ubuntu 22.04 with Android SDK, NDK, and build tools
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

### react-android-linux

**Purpose**: A containerized build environment for React Native applications targeting Android, with full Java and Android SDK support.

**What's Included**:
- Base: Ubuntu 22.04 LTS
- Java: OpenJDK 17 (required for Android development)
- Node.js: v18.x with npm
- Package Manager: Yarn (npm-based)
- Android SDK: Tools, platform-tools, Android API 34, and build-tools
- Utilities: Git, curl, wget, unzip, build-essential

**Quick Start**:
```bash
# Build the image
docker build -t react-android-linux ./react-android-linux

# Run interactively
docker run -it react-android-linux

# Mount local React Native project
docker run -it -v $(pwd):/app react-android-linux
```

**Key Features**:
- Complete Android development setup (SDK, tools, API levels)
- Node.js and Yarn pre-configured for React Native projects
- Automated license acceptance for Android SDK
- Ready for building APK/AAB files
- Properly configured environment variables (ANDROID_HOME, ANDROID_SDK_ROOT)

### vlc-libvlc-jni

**Purpose**: A containerized build environment for compiling libvlc-jni (VLC Android bindings) from source code.

**What's Included**:
- Base: Ubuntu 22.04 LTS
- Java: OpenJDK 17 JDK and JRE
- Android SDK: Command-line tools, platform-tools, build-tools 34.0.0, Android API 34
- Android NDK: Version 29.0.13113456 (C/C++ development support)
- Build tools: build-essential, CMake, Automake, Autoconf, Libtool, Ant, Make
- Assembly tools: YASM, Ragel
- Parsing tools: Flex, M4
- Compilation support: Protobuf compiler, pkg-config
- Utilities: Git, wget, curl, unzip, SVN, Python 3
- 32-bit libraries: For Android SDK compatibility
- Unprivileged user (`builder`) with sudo access

**Quick Start**:
```bash
# Build the image
docker build -t vlc-libvlc-jni ./vlc-libvlc-jni

# Run interactively
docker run -it vlc-libvlc-jni

# Mount local libvlc-jni source for compilation
docker run -it -v $(pwd):/workspace vlc-libvlc-jni
```

**Key Features**:
- Full Android development toolchain (SDK, NDK, build-tools)
- All required C/C++ compilation tools for native Android development
- Pre-configured environment variables (ANDROID_SDK_ROOT, ANDROID_NDK_ROOT, JAVA_HOME)
- 32-bit library support for Android SDK tools compatibility
- Ready to compile VLC JNI bindings for Android platforms

### vlc-build-env

**Purpose**: A containerized build environment for compiling VLC media player from source code.

**What's Included**:
- Base: Ubuntu 22.04 LTS
- Build tools: build-essential, CMake, Meson, Ninja, Autoconf, Automake, Libtool
- Assembly tools: YASM, NASM
- Parsing tools: Flex, Bison
- Multimedia libraries: FFmpeg dev libraries (libavcodec, libavformat, libavutil, libswscale, libavfilter, libswresample)
- Audio support: libasound2-dev (ALSA)
- Security: libssl-dev
- Build utilities: Git, pkg-config

**Quick Start**:
```bash
# Build the image
docker build -t vlc-build-env ./vlc-build-env

# Run interactively
docker run -it vlc-build-env

# Mount local VLC source for compilation
docker run -it -v $(pwd)/vlc:/workspace vlc-build-env
```

**Key Features**:
- Full compilation toolchain for VLC media player
- All required multimedia and codec libraries pre-installed
- ALSA audio support
- OpenSSL for secure connections
- Ready to compile VLC source code with various plugins and features


## What You'll Find Here

This repository documents:

✅ **Custom Dockerfile configurations** - Development environments optimized for specific workflows  
✅ **Docker best practices** - Layer optimization, non-root user setup, versioning strategies  
✅ **Containerization patterns** - Multi-stage builds, volume management, networking (as they're added)  
✅ **Learning notes** - Commentary on design decisions and containerization concepts  

## Future Additions

This repository will grow to include:
- Additional Docker images (Python, full-stack dev environments, etc.)
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

**Last Updated**: March 2026

**Maintainer**: Arun Raj

**Note**: README Generated using AI
