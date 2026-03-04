# Use Ubuntu 24.04 LTS as base
FROM ubuntu:24.04

# Metadata
LABEL maintainer="Arun Raj"
LABEL description="Container for compiling VLC Source code based ong Ubuntu-24.04"

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
    g++ \
    make \
    libtool \
    automake \
    autopoint \
    pkg-config \
    flex \
    bison \
    lua5.2 \
    libavcodec-dev \
    libavformat-dev \
    libswscale-dev \
    libavcodec-extra \
    gettext \
 && apt clean \
 && rm -rf /var/lib/apt/lists/*


# Create the user
ARG USERNAME=devuser
RUN useradd -m -s /bin/bash $USERNAME && \
    echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/$USERNAME && \
    chmod 0440 /etc/sudoers.d/$USERNAME

# Allow devuser user to use sudo if needed
RUN usermod -aG sudo $USERNAME

# --- Switch to the new user for the rest of the setup ---
USER $USERNAME
WORKDIR /home/$USERNAME

RUN echo "git clone https://code.videolan.org/videolan/vlc.git \ncd vlc \n./bootstrap \n./configure --disable-lua --disable-xcb --disable-alsa --disable-qt \nmake" > README.txt


# Re‑enable interactive shell for user
SHELL ["/bin/bash", "--rcfile", "/home/${USERNAME}/.bashrc", "-c"]

# Default command
CMD ["bash"]

