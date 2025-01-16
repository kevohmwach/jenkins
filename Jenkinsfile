pipeline {
    agent { 
        node {
            label 'jenkins-agent-docker'
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