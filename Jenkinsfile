
pipeline {
    agent any

    environment {
        PYTHON = 'C:\\Users\\MADHAN\\AppData\\Local\\Programs\\Python\\Python312\\python.exe'
        DOCKER = 'C:\\Users\\MADHAN\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe'
        KUBECTL = 'C:\\Users\\MADHAN\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\kubectl.exe'
        KUBECONFIG = 'C:\\Users\\MADHAN\\.kube\\config'
    }

    stages {
        stage('1. Checkout') {
            steps {
                checkout scm
            }
        }

        stage('2. Install Dependencies') {
            steps {
                bat '"%PYTHON%" -m pip install -r requirements.txt'
            }
        }

        stage('3. Run Tests') {
            steps {
                bat '"%PYTHON%" -m pytest -v'
            }
        }

        stage('4. Build Docker Image') {
            steps {
                bat '"%DOCKER%" build -t student-k8s-demo:v1 .'
            }
        }

        stage('5. Verify Docker Image') {
            steps {
                bat '"%DOCKER%" image inspect student-k8s-demo:v1'
            }
        }


       stage('6. Deploy to Kubernetes') {
            steps {
                bat '"%KUBECTL%" config current-context'
                bat '"%KUBECTL%" get nodes'
                bat '"%KUBECTL%" apply -f k8s/deployment.yaml'
                bat '"%KUBECTL%" apply -f k8s/service.yaml'
                bat '"%KUBECTL%" rollout status deployment/student-k8s-demo --timeout=120s'
            }
        }


        stage('7. Verify Deployment') {
            steps {
                bat '"%KUBECTL%" get pods'
                bat '"%KUBECTL%" get services'
            }
        }
    }
}
