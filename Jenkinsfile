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
                sh 'docker-compose run --rm app npm test -- --watchAll=false'
            }
        }

        stage('Cleanup') {
            steps {
                sh 'docker-compose down --volumes --remove-orphans'
            }
        }

        stage('Publish Checks') {
            steps {
                script {
                    def checkResult = 'SUCCESS'

                    if (currentBuild.result == 'FAILURE') {
                        checkResult = 'FAILURE'
                    } else if (currentBuild.result == 'UNSTABLE') {
                        checkResult = 'NEUTRAL'
                    } else if (currentBuild.result == 'ABORTED') {
                        checkResult = 'CANCELLED'
                    }

                    publishChecks(
                        name: 'Jenkins Build',
                        title: 'Build Status',
                        summary: "Jenkins build ${currentBuild.result}",
                        conclusion: checkResult,
                        credentialsId: 'your-github-credential-id' // Replace with your credential ID
                    )
                }
            }
        }
    }
}
