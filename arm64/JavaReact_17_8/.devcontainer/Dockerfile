# Use an official base image
FROM mcr.microsoft.com/vscode/devcontainers/base:latest

# Create vscode user and group with the necessary UID and GID if they don't exist
RUN getent group vscode || groupadd -g 1000 vscode && \
    getent passwd vscode || useradd -m -u 1000 -g vscode vscode

# Set the working directory
WORKDIR /workspace

# Set the user to vscode to ensure that subsequent commands are run as this user
USER vscode

# Install OpenJDK (latest version from Ubuntu repositories)
RUN sudo apt-get update && sudo apt-get install -y openjdk-17-jdk

# Set JAVA_HOME environment variable (adjust version based on what you installed)
ENV JAVA_HOME=/usr/lib/jvm/java-17-openjdk-arm64
ENV PATH="$JAVA_HOME/bin:$PATH"

# Install Maven
RUN sudo apt-get install -y maven

# Install Node.js (using NodeSource repository for the latest version)
RUN curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash - && \
    sudo apt-get install -y nodejs

# Install Yarn (package manager for Node.js) with --unsafe-perm to avoid permission issues
# RUN npm install --global --unsafe-perm yarn

# Switch to root to install Yarn globally
USER root

# Install Yarn (package manager for Node.js)
RUN npm install --global yarn

# Revert to the vscode user
USER vscode

# Verify installation of Node.js, NPM, Yarn, and Maven
RUN node -v && npm -v && mvn -v && yarn -v
# RUN node -v && npm -v && mvn -v

# Install React dependencies (ensure package.json is in the /workspace folder)
# COPY package*.json ./
# RUN npm install

# Expose the necessary ports for your application
EXPOSE 3000 8000

# Set the default shell to bash
ENV SHELL=/bin/bash

# The entrypoint could also be a custom script if you need to start something specific
CMD ["bash"]
