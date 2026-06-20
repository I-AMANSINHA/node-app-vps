pipeline {
    agent any

    environment {
        IMAGE_NAME = "node-app"
        CONTAINER_NAME = "node-app"
    }

    stages {

        stage('Git Checkout') {
            steps {
                checkout scm

                sh '''
                echo "Latest Commit:"
                git log -1 --pretty=format:"%h | %an | %s"
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                docker build \
                  -t ${IMAGE_NAME}:${BUILD_NUMBER} .
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker rm -f ${CONTAINER_NAME} || true

                docker run -d \
                  --name ${CONTAINER_NAME} \
                  -p 4000:4000 \
                  --restart unless-stopped \
                  ${IMAGE_NAME}:${BUILD_NUMBER}
                '''
            }
        }
    }
}
