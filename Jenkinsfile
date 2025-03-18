pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("localhost:5000/my-app:")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    sh 'docker push localhost:5000/my-app:'
                }
            }
        }
        stage('Deploy with ArgoCD') {
            steps {
                sh 'kubectl apply -f k8s-manifests/'
            }
        }
    }
}
