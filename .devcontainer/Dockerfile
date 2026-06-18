FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04

# The base image already includes a 'vscode' user with sudo access.
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
    curl \
    git \
    gnupg2 \
    ssh-client \
    wget \
    apt-transport-https \
    ca-certificates \
    libssl-dev \
    openssl \
    libgcc-s1 \
    libstdc++6 \
    zlib1g \
    libxml2-utils \
    && wget -qO - https://packages.adoptium.net/artifactory/api/gpg/key/public \
       | gpg --dearmor | tee /etc/apt/trusted.gpg.d/adoptium.gpg > /dev/null \
    && . /etc/os-release \
    && echo "deb https://packages.adoptium.net/artifactory/deb ${VERSION_CODENAME} main" \
       | tee /etc/apt/sources.list.d/adoptium.list \
    && apt-get update \
    && apt-get -y install --no-install-recommends temurin-8-jdk \
    && find /usr/lib/jvm -maxdepth 1 -name "temurin-8*" -type d -exec ln -sf {} /usr/local/jdk8 \; \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Make Maven use JDK 8 while leaving JDK 21 as the default JVM.
# The 'mvn' script sources $HOME/.mavenrc, so JAVA_HOME here only affects Maven.
RUN echo 'export JAVA_HOME=/usr/local/jdk8' > /home/vscode/.mavenrc \
    && chown vscode:vscode /home/vscode/.mavenrc

# Download Lombok for IDE support
RUN mkdir -p /usr/local/lib \
    && wget -q https://repo1.maven.org/maven2/org/projectlombok/lombok/1.18.34/lombok-1.18.34.jar \
       -O /usr/local/lib/lombok.jar

# Set the default shell to zsh
ENV SHELL=/bin/zsh
