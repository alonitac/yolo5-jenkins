pipeline {
    agent any

    stages {
        stage('Install dependencies') {
            steps {
                sh '''
                pip3 install build twine
                '''
            }
        }
        stage('Build') {
            steps {
                sh '''
                python3 -m build
                '''
            }
        }

        stage('Publish') {
            steps {
                withCredentials([
                    usernamePassword(credentialsId: 'nexus', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')
                ]) {
                    sh '''
                    sed -i -e "s/<username>/$USERNAME/g" .pypirc
                    sed -i -e "s/<password>/$PASSWORD/g" .pypirc
                    python3 -m twine upload --config-file .pypirc --repository pypi-hosted dist/*
                    '''
                }
            }
        }
    }
}