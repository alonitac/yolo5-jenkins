pipeline {

    agent {
        docker {
            image '700935310038.dkr.ecr.eu-north-1.amazonaws.com/alonit-jenkins-agent:latest'
            args  '--user root -v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    stages {
        stage('Unittest') {
            steps {
                echo "testing"
            }
        }
        stage('Lint') {
            steps {
                echo "linting"
            }
        }
        stage('Functional test') {
            steps {
                echo "testing"
            }
        }
    }
}