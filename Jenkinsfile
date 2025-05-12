pipeline {
    agent any

    environment {
        jobname = "${env.JOB_NAME}".split('/').first()
        targetDir = "/home/azizfahriz/frontend"
    }

    stages {
        stage('Clone Repository') {
            steps {
                checkout scm
            }
        }

        stage('Copy to Target Directory') {
            steps {
                sh """
                    cp -r . ${targetDir}
                """
            }
        }

        stage('Start Docker Compose') {
            steps {
                sh """
                    cd ${targetDir}
                    docker-compose down || true
                    docker-compose up -d --build
                """
            }
        }
    }

    post {
        success {
            echo "Pipeline executed successfully."
        }
        failure {
            echo "Pipeline failed."
        }
    }
}
