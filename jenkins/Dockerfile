FROM jenkins/jenkins:lts

ARG GROUP_ID=999
ENV GROUP_ID=${GROUP_ID} \
    DOCKER_VERSION=18.03.1

USER root

RUN mkdir -p /tmp/download && \
    curl -L https://download.docker.com/linux/static/stable/x86_64/docker-${DOCKER_VERSION}-ce.tgz | tar -xz -C /tmp/download && \
    rm -rf /tmp/download/docker/dockerd && \
    mv /tmp/download/docker/docker* /usr/local/bin/ && \
    rm -rf /tmp/download && \
    groupadd -g ${GROUP_ID} docker && \
    usermod -aG staff,docker jenkins

#USER jenkins
