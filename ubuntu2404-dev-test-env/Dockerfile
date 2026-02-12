# Use Ubuntu 24.04 LTS as base
FROM ubuntu:24.04

# Metadata
LABEL maintainer="Arun Raj"
LABEL description="Container for C/C++ code testing and practice with Neovim + NvChad"

# Avoid interactive prompts during apt install
ENV DEBIAN_FRONTEND=noninteractive

USER root

# Install C/C++ build tools and basic dev deps
RUN apt update \
 && apt install --yes --no-install-recommends \
    build-essential \
    clang \
    cmake \
    git \
    curl \
    unzip \
    ca-certificates \
    software-properties-common \
    sudo \
    vim \
 && apt clean \
 && rm -rf /var/lib/apt/lists/*

# Allow ubuntu user to use sudo if needed
RUN usermod -aG sudo ubuntu

# Install Neovim (via official repo) + ripgrep
RUN curl -LO https://github.com/neovim/neovim/releases/download/v0.11.6/nvim-linux-x86_64.tar.gz \
 && tar -xzf nvim-linux-x86_64.tar.gz -C /opt \
 && mkdir -p /usr/local/bin \
 && ln -s /opt/nvim-linux-x86_64/bin/nvim /usr/local/bin/nvim \
 && rm -rf nvim-linux-* \
 # Install ripgrep
 && curl -LO https://github.com/BurntSushi/ripgrep/releases/download/15.1.0/ripgrep-15.1.0-x86_64-unknown-linux-musl.tar.gz \
 && tar -xzf ripgrep-15.1.0-x86_64-unknown-linux-musl.tar.gz \
 && cp ripgrep-15.1.0-x86_64-unknown-linux-musl/rg /usr/local/bin/rg \
 && chmod +x /usr/local/bin/rg \
 && rm -rf ripgrep-15.1.0-x86*

# Create the user
ARG USERNAME=devuser
RUN useradd -m -s /bin/bash $USERNAME && \
    echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/$USERNAME && \
    chmod 0440 /etc/sudoers.d/$USERNAME

# --- Switch to the new user for the rest of the setup ---
USER $USERNAME
WORKDIR /home/$USERNAME


# Create alias: vim → nvim
RUN echo 'alias vim=nvim' >> ~/.bashrc

# Optionally: make ripgrep replace `grep` in shell (uncomment if desired)
RUN echo 'alias grep=rg' >> ~/.bashrc

# Install NvChad (minimal starter config)
RUN git clone https://github.com/NvChad/starter ~/.config/nvim \
 && sed -i '/- starter/c\\- starter' ~/.config/nvim/init.lua \
 && mkdir -p ~/.config/nvim/lua/custom

# Re‑enable interactive shell for user
SHELL ["/bin/bash", "--rcfile", "/home/${USERNAME}/.bashrc", "-c"]

# Optional: expose default C/C++ debug port if needed
# EXPOSE 8000

# Default command
CMD ["bash"]

