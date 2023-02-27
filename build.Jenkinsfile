pipeline {
    agent {
        docker {
            label 'agent-ec2'
            image '700935310038.dkr.ecr.eu-north-1.amazonaws.com/alonit-jenkins-agent:latest'
            args  '--user root -v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    options {
        timeout(time: 1, unit: 'HOURS')
        timestamps()
    }

    environment {
        REGISTRY_URL = '700935310038.dkr.ecr.eu-north-1.amazonaws.com'
        IMAGE_NAME = 'alonit-yolo5'
    }

    stages {
        stage('Build') {
            steps {
                sh '''
                aws ecr get-login-password --region eu-north-1 | docker login --username AWS --password-stdin $REGISTRY_URL
                docker build -t $IMAGE_NAME:$BUILD_NUMBER .
                '''

//                 withCredentials([
//                     string(credentialsId: 'snyk_token', variable: 'SNYK_TOKEN')
//                 ]) {
//                     sh '''
//                       snyk container test $IMAGE_NAME:$BUILD_NUMBER --severity-threshold=high --file=Dockerfile
//                     '''
//                 }


                sh '''
                docker tag $IMAGE_NAME:$BUILD_NUMBER $REGISTRY_URL/$IMAGE_NAME:$BUILD_NUMBER
                docker push $REGISTRY_URL/$IMAGE_NAME:$BUILD_NUMBER
                '''
            }
            post {
               always {
                   sh 'docker image prune -a --filter "until=240h" --force'
               }
            }
        }

        stage('Trigger Deploy') {
            steps {
                build job: 'AppDeploy', wait: false, parameters: [
                    string(name: 'YOLO5_IMAGE_URL', value: "$REGISTRY_URL/$IMAGE_NAME:$BUILD_NUMBER")
                ]
            }
        }
    }
}