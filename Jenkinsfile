pipeline {
    agent any

    triggers {
        pollSCM('H/5 * * * *') // Cek perubahan setiap 5 menit
    }

    environment {
        GIT_REPO = 'https://github.com/fahrizaziz/fahriz-dev.git'  // Public repo
        GIT_BRANCH = 'main'
        REMOTE_PATH = '/home/azizfahriz/app'
        REMOTE_HOST = credentials('remote-host')
        REMOTE_USER = credentials('remote-user')
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: env.GIT_BRANCH, url: env.GIT_REPO
            }
        }

        stage('Copy to Remote') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'server-aziz', keyFileVariable: 'SSH_KEY')]) {
                    sh """
                        scp -i $SSH_KEY -o StrictHostKeyChecking=no -r * ${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_PATH}
                    """
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'server-aziz', keyFileVariable: 'SSH_KEY')]) {
                    sh """
                        ssh -i $SSH_KEY -o StrictHostKeyChecking=no ${REMOTE_USER}@${REMOTE_HOST} '
                            cd ${REMOTE_PATH} && \
                            docker compose down && \
                            docker compose up -d --build
                        '
                    """
                }
            }
        }
    }
}
