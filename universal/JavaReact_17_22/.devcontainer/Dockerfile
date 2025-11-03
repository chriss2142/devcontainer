# Use Eclipse Temurin Java 17 base image (ARM64 compatible)
FROM eclipse-temurin:17-jdk-focal

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive
ENV JAVA_HOME=/opt/java/openjdk
ENV PATH=$JAVA_HOME/bin:$PATH

# Install prerequisites
RUN apt-get update && apt-get install -y \
    curl \
    wget \
    git \
    unzip \
    tar \
    build-essential \
    libgtk-3-0 \
    libxext6 \
    libxrender1 \
    libxtst6 \
    libnss3 \
    libasound2 \
    && rm -rf /var/lib/apt/lists/*

# Install Maven 3.9
RUN wget https://archive.apache.org/dist/maven/maven-3/3.9.0/binaries/apache-maven-3.9.0-bin.tar.gz -O /tmp/maven.tar.gz \
    && tar -xzf /tmp/maven.tar.gz -C /opt/ \
    && rm /tmp/maven.tar.gz
ENV MAVEN_HOME=/opt/apache-maven-3.9.0
ENV PATH=$MAVEN_HOME/bin:$PATH

# Install Node.js 22 and Yarn 1.22
RUN curl -fsSL https://deb.nodesource.com/setup_22.x | bash - \
    && apt-get install -y nodejs \
    && npm install -g yarn@1.22 \
    && rm -rf /var/lib/apt/lists/*

# Set default working directory
WORKDIR /workspace

# Expose port for devcontainer tools
EXPOSE 3000

# Default command
CMD [ "bash" ]
