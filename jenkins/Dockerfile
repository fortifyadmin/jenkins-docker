FROM jenkins/jenkins:lts

ARG GROUP_ID=999

ENV GROUP_ID=${GROUP_ID} \
    DOCKER_VERSION=18.03.1 \
    CASC_JENKINS_CONFIG=/usr/share/jenkins/ref

ENV JAVA_OPTS "-Djenkins.install.runSetupWizard=false -Dhudson.footerURL=http://spruth.co \
    -Djava.util.logging.config.file=/usr/share/jenkins/ref/log.properties ${JAVA_OPTS:-}"

USER root
COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
COPY executors.groovy /usr/share/jenkins/ref/init.groovy.d/executors.groovy
COPY log.properties /usr/share/jenkins/ref/log.properties
COPY jenkins.yaml /usr/share/jenkins/ref/jenkins.yaml

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
