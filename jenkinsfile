pipeline {
    agent any

    environment {
        IMAGE_NAME = "sajj475/app-name"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/Sajjad8036/itachi.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 3000:3000 $IMAGE_NAME'
            }
        }

        stage('Push to Docker Hub') {
            when {
                expression { return env.BRANCH_NAME == "main" }
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                    sh 'docker push $IMAGE_NAME'
                }
            }
        }
    }
}

        always {
            echo "Pipeline finished with status: ${currentBuild.currentResult}"
        }
    }
}
