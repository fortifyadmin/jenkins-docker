FROM jenkins/jenkins:lts

ARG GROUP_ID=999
ARG JENKINS_ADMIN_ID=brandon
ARG JENKINS_ADMIN_PASSWORD=abc123

ENV GROUP_ID=${GROUP_ID} \
    DOCKER_VERSION=18.03.1 \
    _JAVA_OPTIONS="$JAVA_OPTS -Djenkins.install.runSetupWizard=false" \
    CASC_JENKINS_CONFIG=/var/jenkins_home/casc.yaml \
    JENKINS_ADMIN_ID=${JENKINS_ADMIN_ID} \
    JENKINS_ADMIN_PASSWORD=${JENKINS_ADMIN_PASSWORD}

USER root

RUN mkdir -p /tmp/download && \
    curl -L https://download.docker.com/linux/static/stable/x86_64/docker-${DOCKER_VERSION}-ce.tgz | tar -xz -C /tmp/download && \
    rm -rf /tmp/download/docker/dockerd && \
    mv /tmp/download/docker/docker* /usr/local/bin/ && \
    rm -rf /tmp/download && \
    groupadd -g ${GROUP_ID} docker && \
    usermod -aG staff,docker jenkins

COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
COPY casc.yaml /var/jenkins_home/casc.yaml

RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt

#USER jenkins
