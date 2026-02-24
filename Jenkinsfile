pipeline {
    agent any

    environment {
        BACKEND_IMAGE = "springboot-backend"
        FRONTEND_IMAGE = "streamlit-frontend"
        NETWORK = "app-network"
        BACKEND_CONTAINER = "backend"
        FRONTEND_CONTAINER = "frontend"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                url: 'https://github.com/mahimapatel93/Docker-Jenkins-Java-springboot-project.git'
            }
        }

        stage('Build Backend Image') {
            steps {
                dir('backend') {
                    sh 'docker build -t $BACKEND_IMAGE .'
                }
            }
        }

        stage('Build Frontend Image') {
            steps {
                dir('frontend') {
                    sh 'docker build -t $FRONTEND_IMAGE .'
                }
            }
        }

        stage('Create Network') {
            steps {
                sh 'docker network inspect $NETWORK >/dev/null 2>&1 || docker network create $NETWORK'
            }
        }

        stage('Run Backend Container') {
            steps {
                sh '''
                docker stop $BACKEND_CONTAINER || true
                docker rm $BACKEND_CONTAINER || true

                docker run -d \
                --name $BACKEND_CONTAINER \
                --network $NETWORK \
                -p 8084:8084 \
                $BACKEND_IMAGE
                '''
            }
        }

        stage('Run Frontend Container') {
            steps {
                sh '''
                docker stop $FRONTEND_CONTAINER || true
                docker rm $FRONTEND_CONTAINER || true

                docker run -d \
                --name $FRONTEND_CONTAINER \
                --network $NETWORK \
                -p 8501:8501 \
                $FRONTEND_IMAGE
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful!"
        }
        failure {
            echo "❌ Deployment Failed!"
        }
    }
}
