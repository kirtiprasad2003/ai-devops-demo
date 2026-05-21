pipeline {
    agent any

    stages {

        stage('Check Cluster') {
            steps {
                sh 'kubectl get nodes'
            }
        }

        stage('Restart Deployment') {
            steps {
                sh 'kubectl rollout restart deployment fastapi-demo'
            }
        }

        stage('Verify Rollout') {
            steps {
                sh 'kubectl rollout status deployment/fastapi-demo'
            }
        }

        stage('Check Pods') {
            steps {
                sh 'kubectl get pods'
            }
        }

    }
}