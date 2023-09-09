FROM jenkins/jenkins:jdk11

ARG DEBIAN_FRONTEND=noninteractive

ENV JAVA_TOOL_OPTIONS="-Djenkins.install.runSetupWizard=false -Xmx200m"

USER root

RUN apt-get update \
    && apt-get -y install apt-utils \
    && apt-get -y install \
    ca-certificates \
    curl \
    gnupg

RUN install -m 0755 -d /etc/apt/keyrings && \
    ( curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg ) && \
    chmod a+r /etc/apt/keyrings/docker.gpg && \
    ( echo "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
      "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
      tee /etc/apt/sources.list.d/docker.list > /dev/null )

RUN apt-get update \
    && apt-get -y install docker-ce \
    && usermod -aG docker jenkins

RUN mkdir /var/shared \
    && mkdir /var/shared/maven-dir \
    && chown -R 1000:1000 /var/shared

ENV MAVEN_DIR /var/shared/maven-dir

USER jenkins

RUN jenkins-plugin-cli \
    --plugins \
    git \
    workflow-aggregator \
    bitbucket \
    blueocean \
    metrics \
    jobConfigHistory \
    jacoco \
    pipeline-maven \
    gradle \
    warnings-ng \
    google-play-android-publisher \
    docker-workflow \
    code-coverage-api \
    ssh-slaves \
    mysql-api \
    locale \
    git-forensics

VOLUME $MAVEN_DIR