pipeline {
    agent { 
        node {
            // label 'jenkins-docker-slave-ssh'
            label 'jenkins-agent-docker-jnlp'
            }
      }
    triggers {
        pollSCM '*/5 * * * *'
    }
    stages {
        stage('Build') {
            steps {
                echo "Buildingssssssssss.."
                
            }
        }
        stage('Test') {
            steps {
                echo "Testingssss.."
            }
        }
        stage('Deliver') {
            steps {
                echo 'Deliversssssssssssssss....'
            }
        }
    }
}