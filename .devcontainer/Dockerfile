FROM ubuntu:latest

ARG TARGETARCH
ENV AWS_REGION=eu-west-1
ENV TF_VAR_region=$AWS_REGION

# Install dependencies
RUN apt-get update && apt-get install -y \
    curl \
    unzip 

# Install AWS CLI 
RUN if [ "$TARGETARCH" = "arm64" ]; then \
        curl -sL "https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip" -o "awscliv2.zip"; \
    else \
        curl -sL "https://awscli.amazonaws.com/awscli-exe-linux-amd64.zip" -o "awscliv2.zip"; \
    fi \
    && unzip awscliv2.zip && ./aws/install && rm -rf awscliv2.zip aws

# Install eksctl
RUN if [ "$TARGETARCH" = "arm64" ]; then \
        ARCH=arm64; \        
    else \
        ARCH=amd64; \
    fi \
    && PLATFORM="linux_${ARCH}" \
    && curl -sLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_$PLATFORM.tar.gz" \
    && tar -xzf eksctl_$PLATFORM.tar.gz -C /usr/local/bin && rm eksctl_$PLATFORM.tar.gz

# Install Kubernetes 
RUN  curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" \
    && install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# Install HELM
RUN curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 \
    && chmod 700 get_helm.sh \
    && ./get_helm.sh
