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

        stage('Publish Checks') { // This needs to be inside the 'stages' block
            steps {
                script {
                    def checkResult = 'SUCCESS' // Default to success

                    if (currentBuild.result == 'FAILURE') {
                        checkResult = 'FAILURE'
                    } else if (currentBuild.result == 'UNSTABLE') {
                        checkResult = 'NEUTRAL' // Using 'NEUTRAL' for COMPLETED_WITH_ANNOTATIONS
                    } else if (currentBuild.result == 'ABORTED') {
                        checkResult = 'CANCELLED'
                    }

                    publishChecks(
                        name: 'Jenkins Build',
                        title: 'Build Status',
                        summary: "Jenkins build ${currentBuild.result}",
                        conclusion: checkResult,
                        credentialsId: 'github-token'
                    )
                }
            }
        }
    }
}
