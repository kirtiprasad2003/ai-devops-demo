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
            --data '{
                "text":"🚀 *AI-DevOps Deployment Successful*",
                "attachments":[
                    {
                        "color":"#36a64f",
                        "fields":[
                            {
                                "title":"Project",
                                "value":"AI DevOps Platform",
                                "short":true
                            },
                            {
                                "title":"Environment",
                                "value":"Kubernetes Local Cluster",
                                "short":true
                            },
                            {
                                "title":"Status",
                                "value":"✅ Deployment Completed Successfully",
                                "short":false
                            },
                            {
                                "title":"Triggered By",
                                "value":"Jenkins CI/CD Pipeline",
                                "short":false
                            }
                        ],
                        "footer":"Enlight DevOps Automation",
                        "footer_icon":"https://cdn-icons-png.flaticon.com/512/5968/5968705.png"
                    }
                ]
            }' \
            $SLACK_WEBHOOK
            """
        }

        failure {
            sh """
            curl -X POST -H 'Content-type: application/json' \
            --data '{
                "text":"🚨 *AI-DevOps Deployment Failed*",
                "attachments":[
                    {
                        "color":"#ff0000",
                        "fields":[
                            {
                                "title":"Project",
                                "value":"AI DevOps Platform",
                                "short":true
                            },
                            {
                                "title":"Environment",
                                "value":"Kubernetes Local Cluster",
                                "short":true
                            },
                            {
                                "title":"Status",
                                "value":"❌ Deployment Failed",
                                "short":false
                            },
                            {
                                "title":"Action Required",
                                "value":"Check Jenkins Console Logs Immediately",
                                "short":false
                            }
                        ],
                        "footer":"Enlight DevOps Automation",
                        "footer_icon":"https://cdn-icons-png.flaticon.com/512/5968/5968705.png"
                    }
                ]
            }' \
            $SLACK_WEBHOOK
            """
        }

    }
}