pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test') {
            steps {
                withChecks(name: 'Tests', title: 'Unit Tests', includeStage: true, credentialsId: 'github-token') {
                    sh 'docker-compose run --rm app npm test -- --watchAll=false'
                }
            }
        }

        stage('Cleanup') {
            steps {
                sh 'docker-compose down --volumes --remove-orphans'
            }
        }
    }
}
