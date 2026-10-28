
pipeline {
    agent any

    stages {
        stage('1. Checkout') {
            steps {
                checkout scm
            }
        }

        stage('2. Install Dependencies') {
            steps {
                bat 'python -m pip install -r requirements.txt'
            }
        }

        stage('3. Run Tests') {
            steps {
                bat 'python -m pytest -v'
            }
        }

        stage('4. Build Docker Image') {
            steps {
                bat 'docker build -t student-k8s-demo:v1 .'
            }
        }

        stage('5. Verify Docker Image') {
            steps {
                bat 'docker image inspect student-k8s-demo:v1'
            }
        }

        stage('6. Deploy to Kubernetes') {
            steps {
                bat 'kubectl apply -f k8s/deployment.yaml'
                bat 'kubectl apply -f k8s/service.yaml'
                bat 'kubectl rollout status deployment/student-k8s-demo --timeout=120s'
            }
        }

        stage('7. Verify Deployment') {
            steps {
                bat 'kubectl get pods'
                bat 'kubectl get services'
            }
        }
    }
}
