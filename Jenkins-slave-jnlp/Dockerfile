FROM jenkins/inbound-agent:latest-jdk11

USER root
RUN apt-get update
RUN apt-get install git-ftp

USER jenkins

ENTRYPOINT ["jenkins-slave"]