pipeline {
    agent any

    environment {
        // Sets the port to 4000 for the environment
        PORT = '4000'
    }

    stages {
        stage('Install Dependencies') {
            steps {
                echo 'Installing node modules...'
                // 'npm ci' is faster and safer than 'npm install' for automation
                sh 'npm ci' 
            }
        }

        stage('Deploy & Run') {
            steps {
                echo 'Starting application on port 4000...'
                // The 'nohup' and '&' run the server in the background 
                // so the Jenkins build can finish without hanging forever
                sh 'BUILD_ID=dontKillMe nohup npm start > app.log 2>&1 &'
            }
        }
    }
}

