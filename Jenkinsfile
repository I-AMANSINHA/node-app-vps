pipeline {
    agent any

    environment {
        IMAGE_NAME = "node-app"
        CONTAINER_NAME = "node-app"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Image') {
            steps {
                sh """
                docker build \
                  -t ${IMAGE_NAME}:${BUILD_NUMBER} .
      
                """
            }
        }

        stage('Deploy') {
            steps {
                sh """
                docker rm -f ${CONTAINER_NAME} || true

                docker run -d \
                  --name ${CONTAINER_NAME} \
                  --restart unless-stopped \
                  -p 4000:4000 \
                  -e BUILD_NUMBER=${BUILD_NUMBER} \
                  ${IMAGE_NAME}:${BUILD_NUMBER}
                """
            }
        }

        stage('Verify') {
            steps {
                sh '''
                docker ps
                '''
            }
        }
    }

    post {
        success {
            echo "Deployment Successful"
        }

        failure {
            echo "Deployment Failed"
        }
    }
}
