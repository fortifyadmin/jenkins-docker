FROM jenkins/jenkins:lts

ARG GROUP_ID=999
ARG JENKINS_ADMIN_ID=brandon
ARG JENKINS_ADMIN_PASSWORD=abc123

ENV GROUP_ID=${GROUP_ID} \
    DOCKER_VERSION=18.03.1 \
    JENKINS_ADMIN_ID=${JENKINS_ADMIN_ID} \
    JENKINS_ADMIN_PASSWORD=${JENKINS_ADMIN_PASSWORD}

ENV JAVA_OPTS "-Djenkins.install.runSetupWizard=false -Dhudson.footerURL=http://spruth.co \
    -Djava.util.logging.config.file=/var/jenkins_home/log.properties ${JAVA_OPTS:-}" 

USER root
COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
COPY jenkins.yaml /var/jenkins_home/jenkins.yaml
COPY executors.groovy /usr/share/jenkins/ref/init.groovy.d/executors.groovy
COPY log.properties /var/jenkins_home/log.properties

# groupadd and usermod are not required, future feature of running service as jenkins user with hosted docker service
RUN mkdir -p /tmp/download && \
    curl -L https://download.docker.com/linux/static/stable/x86_64/docker-${DOCKER_VERSION}-ce.tgz | tar -xz -C /tmp/download && \
    rm -rf /tmp/download/docker/dockerd && \
    mv /tmp/download/docker/docker* /usr/local/bin/ && \
    rm -rf /tmp/download && \
    groupadd -g ${GROUP_ID} docker && \
    usermod -aG staff,docker jenkins

RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt

#USER jenkins
