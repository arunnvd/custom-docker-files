FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

ENV ANDROID_SDK_ROOT=/opt/android-sdk
ENV ANDROID_SDK=/opt/android-sdk
ENV JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64

ENV PATH=$JAVA_HOME/bin:$ANDROID_SDK_ROOT/cmdline-tools/latest/bin:$ANDROID_SDK_ROOT/platform-tools:$ANDROID_SDK_ROOT/tools:$ANDROID_SDK_ROOT/tools/bin:$PATH

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN dpkg --add-architecture i386 \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
        automake \
        ant \
        autopoint \
        build-essential \
        ca-certificates \
        cmake \
        curl \
        file \
        flex \
        gawk \
        git \
        lib32stdc++6 \
        libc6:i386 \
        libstdc++6:i386 \
        libtool-bin \
        m4 \
        make \
        openjdk-17-jdk \
        openjdk-17-jre \
        patch \
        pkg-config \
        protobuf-compiler \
        python-is-python3 \
        ragel \
        subversion \
        unzip \
        wget \
        yasm \
        zlib1g:i386 \
        sudo \
        python3.10-venv \
    && rm -rf /var/lib/apt/lists/*


# Download Android command line tools
RUN mkdir -p ${ANDROID_SDK_ROOT}/cmdline-tools /opt/downloads \
    && cd /opt/downloads \
    && wget -O commandlinetools-linux.zip https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip \
    && unzip commandlinetools-linux.zip -d ${ANDROID_SDK_ROOT}/cmdline-tools \
    && mv ${ANDROID_SDK_ROOT}/cmdline-tools/cmdline-tools ${ANDROID_SDK_ROOT}/cmdline-tools/latest \
    && rm -rf /opt/downloads


# Accept licenses and install Android SDK + NDK
RUN mkdir -p /root/.android \
    && touch /root/.android/repositories.cfg \
    && sdkmanager --sdk_root=${ANDROID_SDK_ROOT} --update \
    && printf 'y\n%.0s' {1..100} | sdkmanager --sdk_root=${ANDROID_SDK_ROOT} --licenses \
    && sdkmanager --sdk_root=${ANDROID_SDK_ROOT} \
        "platform-tools" \
        "build-tools;34.0.0" \
        "platforms;android-34" \
        "ndk;29.0.13113456"


# Set NDK environment
ENV ANDROID_NDK_ROOT=$ANDROID_SDK_ROOT/ndk/29.0.13113456
ENV ANDROID_NDK=$ANDROID_SDK_ROOT/ndk/29.0.13113456

ENV PATH=$PATH:$ANDROID_NDK_ROOT

ARG USERNAME=builder

# Optional sanity check (helps debug CI builds)
RUN java -version \
    && sdkmanager --list > /dev/null \
    && $ANDROID_NDK_ROOT/ndk-build --version

RUN useradd --create-home --shell /bin/bash --uid 1000 $USERNAME \
    && echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME \
    && mkdir -p /workspace \
    && chown -R $USERNAME:$USERNAME /workspace ${ANDROID_SDK_ROOT} ${ANDROID_NDK_ROOT}

RUN usermod -aG sudo $USERNAME

USER $USERNAME
WORKDIR /workspace

#RUN printf '%s\n' \
#    'export ANDROID_SDK=/opt/android-sdk' \
#    'export ANDROID_SDK_ROOT=/opt/android-sdk' \
#    'export ANDROID_NDK=/opt/android-ndk-r21e' \
#    'export ANDROID_NDK_ROOT=/opt/android-ndk-r21e' \
#    'export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64' \
#    'export PATH=/opt/android-sdk/cmdline-tools/latest/bin:/opt/android-sdk/platform-tools:/opt/android-sdk/tools:/opt/android-sdk/tools/bin:/opt/android-ndk-r21e:$PATH' \
#    >> /home/builder/.bashrc

CMD ["/bin/bash"]
