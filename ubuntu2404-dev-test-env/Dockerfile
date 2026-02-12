# Use Ubuntu 24.04 LTS as base
FROM ubuntu:24.04

# Metadata
LABEL maintainer="you@example.com"
LABEL description="Container for C/C++ code testing and practice with Neovim + NvChad"

# Avoid interactive prompts during apt install
ENV DEBIAN_FRONTEND=noninteractive

# Set non‑root user (security)
ENV USER=dev
ENV UID=1000
ENV GID=1000

# Create non‑root user and group
RUN groupadd --gid ${GID} ${USER} \
 && useradd --create-home --uid ${UID} --gid ${GID} ${USER} \
 && usermod -aG sudo ${USER}

# Switch to non‑root for remaining work
USER ${USER}
WORKDIR /home/${USER}

# Install C/C++ build tools and basic dev deps
RUN sudo apt update \
 && sudo apt install --yes --no-install-recommends \
    build-essential \
    clang \
    cmake \
    git \
    curl \
    unzip \
    ca-certificates \
    software-properties-common \
 && sudo apt clean \
 && sudo rm -rf /var/lib/apt/lists/*

# Install Neovim (via official repo) + ripgrep
RUN curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim-linux64.tar.gz \
 && tar -xzf nvim-linux64.tar.gz \
 && sudo mkdir -p /usr/local/bin \
 && sudo cp nvim-linux64/bin/nvim /usr/local/bin/nvim \
 && sudo chmod +x /usr/local/bin/nvim \
 && rm -rf nvim-linux64* \
 # Install ripgrep
 && curl -LO https://github.com/BurntSushi/ripgrep/releases/latest/download/ripgrep-x86_64-unknown-linux-musl.tar.gz \
 && tar -xzf ripgrep-x86_64-unknown-linux-musl.tar.gz \
 && sudo cp rg-x86_64-unknown-linux-musl/rg /usr/local/bin/rg \
 && sudo chmod +x /usr/local/bin/rg \
 && rm -rf rg-x86_64-unknown-linux-musl*

# Create alias: vim → nvim
RUN echo 'alias vim=nvim' >> ~/.bashrc

# Optionally: make ripgrep replace `grep` in shell (uncomment if desired)
# RUN echo 'alias grep=rg' >> ~/.bashrc

# Install NvChad (minimal starter config)
RUN git clone https://github.com/NvChad/starter ~/.config/nvim \
 && sed -i '/- starter/c\\- starter' ~/.config/nvim/init.lua \
 && mkdir -p ~/.config/nvim/lua/custom

# Re‑enable interactive shell for user
SHELL ["/bin/bash", "--rcfile", "/home/${USER}/.bashrc", "-c"]

# Optional: expose default C/C++ debug port if needed
# EXPOSE 8000

# Default command
CMD ["bash"]

