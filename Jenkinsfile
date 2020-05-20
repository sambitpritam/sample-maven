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
    }
}