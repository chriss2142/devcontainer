# Use an official base image
FROM mcr.microsoft.com/vscode/devcontainers/base:latest

# Create vscode user and group with the necessary UID and GID if they don't exist
RUN getent group vscode || groupadd -g 1000 vscode && \
    getent passwd vscode || useradd -m -u 1000 -g vscode vscode

# Set the working directory
WORKDIR /workspace

# Set the user to vscode to ensure that subsequent commands are run as this user
USER root

# Install required dependencies for OpenJDK 24
RUN apt-get update && \
    apt-get install -y \
    wget \
    unzip \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Set OpenJDK 24 version (adjust the URL to the latest JDK 24 version)
RUN wget https://download.java.net/java/GA/jdk24.0.1/24a58e0e276943138bf3e963e6291ac2/9/GPL/openjdk-24.0.1_linux-aarch64_bin.tar.gz -P /tmp && \
    tar -xvf /tmp/openjdk-24.0.1_linux-aarch64_bin.tar.gz -C /opt && \
    rm /tmp/openjdk-24.0.1_linux-aarch64_bin.tar.gz

# Set environment variables for OpenJDK 24
ENV JAVA_HOME=/opt/jdk-24.0.1
ENV PATH=$JAVA_HOME/bin:$PATH

# Install Maven
# RUN sudo apt-get install -y maven

# Install maven as root
USER root

# Download Apache Maven 3.9.9
RUN curl -o /tmp/apache-maven-3.9.9-bin.tar.gz https://dlcdn.apache.org/maven/maven-3/3.9.9/binaries/apache-maven-3.9.9-bin.tar.gz

# Extract the tar.gz file to /opt
RUN mkdir /opt/maven && \
    tar -xvzf /tmp/apache-maven-3.9.9-bin.tar.gz -C /opt/maven --strip-components=1

# Set Maven environment variables
ENV MAVEN_HOME=/opt/maven
ENV PATH=$MAVEN_HOME/bin:$PATH

# Revert to the vscode user
USER vscode

# Download and install nvm:
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.2/install.sh | bash
RUN export NVM_DIR="$HOME/.nvm" && \
  [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" && \
  [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" && \
  nvm install 22 && \
  nvm alias default 22

# Ensure corepack is available and then enable Yarn 4
RUN export NVM_DIR="$HOME/.nvm" && \
  [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" && \
  [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" && \
  nvm use default && \
  corepack prepare yarn@4 --activate && \
  corepack enable yarn

# Set required environment variables for vscode user
ENV JAVA_HOME=/opt/jdk-24.0.1
ENV PATH=$JAVA_HOME/bin:$PATH
ENV MAVEN_HOME=/opt/maven
ENV PATH=$MAVEN_HOME/bin:$PATH

# Install React dependencies (ensure package.json is in the /workspace folder)
# COPY package*.json ./
# RUN npm install

# Expose the necessary ports for your application
EXPOSE 3000 8000

# Set the default shell to bash
ENV SHELL=/bin/bash

# The entrypoint could also be a custom script if you need to start something specific
CMD ["bash"]
