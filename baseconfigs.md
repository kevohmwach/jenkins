# Dockerfile to install jenkins + blue ocean
FROM jenkins/jenkins:2.479.3-jdk17
USER root
RUN apt-get update && apt-get install -y lsb-release
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
  https://download.docker.com/linux/debian/gpg
RUN echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
  https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
RUN apt-get update && apt-get install -y docker-ce-cli
USER jenkins
RUN jenkins-plugin-cli --plugins "blueocean docker-workflow"


# build  Image
docker build -t jenkins-blueocean:2.479.3-1 .

# create network
docker network create jenkins

# Run docker image
    docker run \
    --name jenkins-blueocean \
    --restart=on-failure \
    --detach \
    --network jenkins \
    --env DOCKER_HOST=tcp://docker:2376 \
    --env DOCKER_CERT_PATH=/certs/client \
    --env DOCKER_TLS_VERIFY=1 \
    --publish 8080:8080 \
    --publish 50000:50000 \
    --volume jenkins-data:/var/jenkins_home \
    --volume jenkins-docker-certs:/certs/client:ro \
    jenkins-blueocean:2.479.3-1

# Get initial admin pass from container
  sudo docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword

# Install socat proxy container between host and jenkin container
  docker run -d --restart=always -p 127.0.0.1:2376:2375 --network jenkins -v /var/run/docker.sock:/var/run/docker.sock alpine/socat tcp-listen:2375,fork,reuseaddr unix-connect:/var/run/docker.sock

# Get Container image
  docker inspect <container_id> | grep IPAddress

# Jenkins alpine jnlp slave with git-ftp installed


# *******************************************************************************************
# Pull image online (Run slave node)
docker run --init jenkins/inbound-agent -url http://jenkins-blueocean:8080 -workDir=/home/jenkins da63e6ddc356d25cdc1fc9e347b1a0bf1752025e8c7798dcae13f3b54a4e4523 jenkins-slave-jdk-ftp

# connect container to jenkins network
docker network connect jenkins <88240bb0cc9b>
# **********************************************************************************************


# Set up jenkins slave node - localhost container
curl -sO jenkins-blueocean:8080/jnlpJars/agent.jar
java -jar agent.jar -url http://jenkins-blueocean:8080/ -secret da63e6ddc356d25cdc1fc9e347b1a0bf1752025e8c779
8dcae13f3b54a4e4523 -name "jenkins-slave-jdk-ftp" -webSocket -workDir "/home/jenkins"

# using local built in image -- Not working properly (Git-ftp installed)
docker run --init jenkins-slave-jdk-
 -url http://jenkins-blueocean:8080 -workDir=/home/jenkins da63e6ddc356d25cdc1fc9e347b1a0bf1752025e8c7798dcae13f3b54a4e4523 jenkins-slave-jdk-ftp


git ftp catchup
git ftp push
