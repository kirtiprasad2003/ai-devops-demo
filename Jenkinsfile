pipeline {
    agent any

    environment {
        SLACK_WEBHOOK = credentials('slack-webhook')
    }

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

    post {

        success {
            sh """
            curl -X POST -H 'Content-type: application/json' \
            --data '{"text":"✅ Jenkins Deployment Successful 🚀"}' \
            $SLACK_WEBHOOK
            """
        }

        failure {
            sh """
            curl -X POST -H 'Content-type: application/json' \
            --data '{"text":"❌ Jenkins Deployment Failed"}' \
            $SLACK_WEBHOOK
            """
        }

    }
}