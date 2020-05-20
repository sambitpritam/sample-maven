pipeline {
    agent any

    tools {
        maven 'localMVN'
        jdk 'localJDK'
    }

    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }

        stage ('Build') {
            steps {
                sh 'mvn clean package'
            }

            post {
                success {
                    echo 'Archiving Artifacts...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deploy to Staging') {
            steps {
                build job: 'deploy-to-staging'
            }
        }

        stage ('Deploy to Production') {
            steps {
                timeout(time: 5, unit: 'DAYS') {
                    input message: 'Approve PRODUCTION Deployment'
                    // ,submitter: xxxx [name of the person or group of person who can approve the build]
                }
                
                build job: 'deploy-to-prod'
            }
            post {
                success {
                    echo 'Code deployed to PRODUCTION'
                }
                failure {
                    echo 'Deployment to PRODUCTION environment failed'
                }
            }
        }
    }
}