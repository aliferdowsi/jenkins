# Use OpenJDK 21 slim image as the base
FROM openjdk:21-slim

# Set environment variables for Maven
ENV MAVEN_VERSION=3.9.4
ENV MAVEN_HOME=/opt/apache-maven-${MAVEN_VERSION}
ENV PATH=${MAVEN_HOME}/bin:${PATH}

# Install necessary packages: Git, curl, and other dependencies
RUN apt-get update && \
    apt-get install -y git curl tar && \
    # Download and install Maven
    curl -O https://archive.apache.org/dist/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz && \
    tar -xvzf apache-maven-${MAVEN_VERSION}-bin.tar.gz -C /opt/ && \
    rm apache-maven-${MAVEN_VERSION}-bin.tar.gz && \
    ln -s /opt/apache-maven-${MAVEN_VERSION} /opt/maven && \
    # Clean up
    apt-get clean && rm -rf /var/lib/apt/lists/* && \
    # safe directory permission
    git config --global --add safe.directory "*"

# Create jenkins user (optional, but recommended for better security)
RUN useradd -m -d /home/jenkins -s /bin/bash jenkins
USER jenkins
WORKDIR /home/jenkins
