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
    }

        stage('Publish Checks') {
            steps {
                script {
                    def checkResult = 'SUCCESS' // Default to success

                    if (currentBuild.result == 'FAILURE') {
                        checkResult = 'FAILURE'
                    } else if (currentBuild.result == 'UNSTABLE') {
                        checkResult = 'COMPLETED_WITH_ANNOTATIONS' // Or another appropriate status
                    } else if (currentBuild.result == 'ABORTED') {
                        checkResult = 'CANCELLED'
                    }

                    publishChecks(
                        name: 'Jenkins Build',
                        title: 'Build Status',
                        summary: "Jenkins build ${currentBuild.result}",
                        conclusion: checkResult.replace('SUCCESS', 'SUCCESS').replace('FAILURE', 'FAILURE').replace('COMPLETED_WITH_ANNOTATIONS', 'NEUTRAL').replace('CANCELLED', 'CANCELLED'),
                        credentialsId: 'github-pat-credential' 
                    )
                }
            }
        }
}
