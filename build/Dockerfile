FROM centos:7
LABEL maintainer="mnairn@redhat.com"

ENV OPERATOR_SDK_VERSION=v0.15.1 \
    OC_VERSION="4.5" \
    GOFLAGS="" \
    PATH=$PATH:/usr/local/go/bin

COPY resources/rhit-root-ca.crt /etc/pki/ca-trust/source/anchors/

RUN set -o pipefail && \
    INSTALL_PKGS="make rsync wget" && \
    yum install -y --setopt=tsflags=nodocs $INSTALL_PKGS && \
    curl -Ls https://mirror.openshift.com/pub/openshift-v4/clients/oc/$OC_VERSION/linux/oc.tar.gz | tar -zx && \
    mv oc /usr/local/bin && \
    curl -Lo /usr/local/bin/yq https://github.com/mikefarah/yq/releases/download/3.1.2/yq_linux_amd64 && \
    chmod +x /usr/local/bin/yq && \
    curl -Lo /usr/local/bin/jq https://github.com/stedolan/jq/releases/download/jq-1.6/jq-linux64 && \
    chmod +x /usr/local/bin/jq && \
    wget https://golang.org/dl/go1.15.5.linux-amd64.tar.gz && \
    tar -zxvf go1.15.5.linux-amd64.tar.gz -C /usr/local/

# install operator-sdk
RUN curl -LO https://github.com/operator-framework/operator-sdk/releases/download/${OPERATOR_SDK_VERSION}/operator-sdk-${OPERATOR_SDK_VERSION}-x86_64-linux-gnu && \
    mv operator-sdk-${OPERATOR_SDK_VERSION}-x86_64-linux-gnu /usr/bin/operator-sdk && \
    chmod +x /usr/bin/operator-sdk

COPY scripts/prow /usr/local/bin
COPY scripts/delorean /usr/local/bin
COPY scripts/ocm /usr/local/bin
COPY scripts/jenkins-reports /usr/local/bin
COPY build/delorean /usr/local/bin/delorean
COPY configurations /etc/delorean/configurations
RUN chmod -R +r /etc/delorean/configurations
RUN update-ca-trust
