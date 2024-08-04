# Use the official Ubuntu 22.04 as a base image
FROM ubuntu:22.04

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install necessary packages
RUN apt-get update && apt-get install -y \
    sudo \
    vim \
    curl \
    gnupg \
    lsb-release \
    software-properties-common \
    net-tools \
    iputils-ping \
    && rm -rf /var/lib/apt/lists/*

# Install Docker inside the container (optional, if needed)
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - \
    && add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" \
    && apt-get update && apt-get install -y docker-ce docker-ce-cli containerd.io

# Add your project-specific setup here
# For example, install ORAN-related packages, copy project files, etc.
# RUN apt-get install -y oran-related-packages
# COPY . /path/to/project

# Expose necessary ports (if any)
# EXPOSE 1234

# Define entry point
# CMD ["/path/to/executable"]
