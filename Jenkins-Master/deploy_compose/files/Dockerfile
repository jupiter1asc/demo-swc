FROM jenkins/jenkins:2.263.1-lts-slim
USER root
RUN apt-get update && apt-get install -y apt-transport-https \
       ca-certificates curl gnupg2 \
       software-properties-common \
       mc
RUN curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -
RUN apt-key fingerprint 0EBFCD88
RUN add-apt-repository \
       "deb [arch=amd64] https://download.docker.com/linux/debian \
       $(lsb_release -cs) stable"
RUN apt-get update && apt-get install -y docker-ce-cli
USER jenkins
RUN jenkins-plugin-cli --plugins blueocean:1.24.3             \
 && jenkins-plugin-cli --plugins ansible:1.1                  \
 && jenkins-plugin-cli --plugins ansicolor:0.7.3              \
 && jenkins-plugin-cli --plugins maven-plugin:3.8             \
 && jenkins-plugin-cli --plugins pipeline-maven:3.9.3         \
 && jenkins-plugin-cli --plugins pipeline-utility-steps:2.6.1 \
 && jenkins-plugin-cli --plugins email-ext:2.80               \
 && jenkins-plugin-cli --plugins pipeline-github-lib:1.0      \
 && jenkins-plugin-cli --plugins github:1.32.0                \
 && jenkins-plugin-cli --plugins github-branch-source:2.9.3