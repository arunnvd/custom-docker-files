# Use Ubuntu 24.04 as the base image
FROM ubuntu:24.04

# Set non-interactive mode to avoid manual prompts during installation
ENV DEBIAN_FRONTEND=noninteractive

# Update and install requested packages (radare2, dumpelf, elfls, readelf, objdump, vim, lddtree, gcc)
RUN dpkg --add-architecture i386 && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
    sudo passwd \
    vim \
    wget \
    curl \
    ripgrep \
    fd-find \
    lnav \
    ca-certificates && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Create a new user with root permissions
ARG USERNAME=logviewer
RUN useradd -m -s /bin/bash $USERNAME && \
    echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/$USERNAME && \
    chmod 0440 /etc/sudoers.d/$USERNAME

# Set the default user and working directory
USER $USERNAME
WORKDIR /home/$USERNAME


WORKDIR /home/$USERNAME/logs
RUN chown $USERNAME:$USERNAME /home/$USERNAME/logs

# Entry point
CMD ["/bin/bash"]
