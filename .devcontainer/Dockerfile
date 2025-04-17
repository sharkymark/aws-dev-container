# Use Ubuntu as base image
FROM ubuntu:20.04

ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=UTC

# Install kubectl, curl, etc.
RUN apt-get update && apt-get install -y \
    curl \
    git \
    unzip \
    gzip \
    jq \
    python3-pip \
    groff \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean 

# Install AWS CLI v2
RUN apt-get update && apt-get install -y unzip curl && \
    curl -sSL "https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip" -o "awscliv2.zip" && \
    unzip awscliv2.zip && ./aws/install && rm -rf aws awscliv2.zip

# Install AWS EKS CLI
RUN curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_Linux_arm64.tar.gz" | \
    tar xz -C /usr/local/bin

# Install Kubectl
RUN LATEST_KUBECTL_VERSION=$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt) && \
    curl -sSL "https://dl.k8s.io/release/${LATEST_KUBECTL_VERSION}/bin/linux/arm64/kubectl" -o /usr/local/bin/kubectl && \
    chmod +x /usr/local/bin/kubectl

# Install Helm
RUN LATEST_HELM_VERSION=$(curl -s https://api.github.com/repos/helm/helm/releases/latest | jq -r '.tag_name') && \
    curl -sSL "https://get.helm.sh/helm-${LATEST_HELM_VERSION}-linux-arm64.tar.gz" -o helm.tar.gz && \
    tar -xvzf helm.tar.gz && \
    mv linux-arm64/helm /usr/local/bin/helm && \
    rm -rf helm.tar.gz linux-arm64

WORKDIR /workspaces