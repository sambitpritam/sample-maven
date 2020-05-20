pipeline {
    agent any

    tools {
        localMVN 'Maven 3.6.3'
        localJDK 'jdk8'
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
    }
}